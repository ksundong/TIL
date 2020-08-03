# Docker로 ELK스택 설치하기

## Docker를 사용하는 이유

1. Docker를 이용할 수 있는 모든 플랫폼에서 동일한 방식으로 적용이 가능합니다.
2. 맥에다가 설치했다가 지우는게 귀찮아서 Docker로 깔끔하게 사용하고자 합니다.
3. VM 서비스에 비해서 설치가 간단한데, 심지어 사이드 이펙트도 없습니다.

### 다른 대안

호스팅형 서비스인 Elastic Service 무료 체험판을 활용하는 것도 도움이 될 것 같습니다.

## ELK 스택이란?

[예쁜 공식 사이트 참고](https://www.elastic.co/kr/what-is/elk-stack) (귀엽게 사슴뿔같은게 있어서 보니까 elk라는 동물에서 따온 모양입니다.)

**E**lasticsearch, **L**ogstash, **K**ibana 를 통칭하는 말.

Elasticsearch: JSON 기반의 분산형 오픈 소스 검색 및 분석 엔진, 주로 REST API를 통해 처리합니다.

Logstash: 여러 소스에서 동시에 데이터를 수집하여 변환한 후, Elasticsearch같은 "stash"로 전송하는 서버사이드 데이터 처리 파이프라인

Kibana: 사용자가 Elasticsearch에서 차트와 그래프를 이용해 데이터를 시각화 할 수 있게 해줌.

거기에 더해 Beat(경량의 단일 목적 데이터 수집기)를 추가했습니다.

그래서 최종적으로 ElasticStack이 되었다고 합니다.

[Elastic Stack의 기능 공식 문서](https://www.elastic.co/kr/elastic-stack/features)

근데 어차피 여기서 우리가 쓸건 Elasticsearch와 Kibana가 대부분일 것 같습니다.

### (+) X-Pack: Elastic Stack의 확장팩

![x-pack-features](https://i.imgur.com/TDbn6k6.png) 
[이미지 출처](https://17billion.github.io/elastic/2017/06/30/elastic_stack_overview.html)

Monitoring 기능을 제외하고는 모두 유료...

따라서 여기선 사용하지 않습니다.

Security: 인증기능, 사용자 관리, 노드/HTTP/Elasticsearch 클라이언트 간 통신 트래픽 보호, Field/Document 수준까지 데이터 보호, Audit Log(감시로그) 기능 제공

Alerting: 데이터 변경사항에 대한 알림 제공

Monitoring: Elastic Stack상태를 지속적으로 체크

Reporting: PDF 형식의 주기적인 보고서를 생성, Email 등으로 전송

Graph: 데이터 시각화

Machine Learning: 데이터의 흐름, 주기성 등을 자동으로 실시간 모니터링을 하여 문제를 식별하고 근본원인 분석

## ELK 스택 설치하기(Github Repository 이용)

https://github.com/deviantony/docker-elk 이 레포지토리를 클론해서 사용할 생각입니다.

```shell
git clone https://github.com/deviantony/docker-elk.git
cd docker-elk
```

### 설정 변경

X-pack을 사용하지 않을 것이기 때문에 제거해줍니다.

1. Elasticsearch

```shell
vi elasticsearch/config/elasticsearch.yml
```

```yaml
---
## Default Elasticsearch configuration from Elasticsearch base image.
## https://github.com/elastic/elasticsearch/blob/master/distribution/docker/src/docker/config/el    asticsearch.yml
#
cluster.name: "docker-cluster"
network.host: 0.0.0.0

```

```shell
vi elasticsearch/Dockerfile
```

```dockerfile
ARG ELK_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/elasticsearch/elasticsearch:${ELK_VERSION}

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu
RUN elasticsearch-plugin install analysis-nori

```

한글 분석기 nori를 설치합니다.

1. Kibana

```shell
vi kibana/config/kibana.yml
```

```yaml
---
## Default Kibana configuration from kibana-docker.
## https://github.com/elastic/kibana-docker/blob/master/.tedi/template/kibana.yml.j2
#
server.name: kibana
server.host: "0"
elasticsearch.hosts: [ "http://elasticsearch:9200" ]
xpack.monitoring.ui.container.elasticsearch.enabled: true
```

3. Logstash

```shell
vi logstash/config/logstash.yml
```

```yaml
---
## Default Logstash configuration from logstash-docker.
## from https://github.com/elastic/logstash-docker/blob/master/build/logstash/config/logstash-full.yml
#
http.host: "0.0.0.0"
xpack.monitoring.elasticsearch.hosts: [ "http://elasticsearch:9200" ]
```

```shell
vi logstash/pipeline/logstash.conf
```

```conf
input {
        tcp {
                port => 5000
        }
}

## Add your filters / logstash plugins configuration here

output {
        elasticsearch {
                hosts => "elasticsearch:9200"
                index => "logstash-20200803"
                user => "username"
                password => "password"
        }
}
```

4. docker-compose

```shell
vi docker-compose.yml
```

```yaml
version: '3.2'

services:
  elasticsearch:
    build:
      context: elasticsearch/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./elasticsearch/config/elasticsearch.yml
        target: /usr/share/elasticsearch/config/elasticsearch.yml
        read_only: true
      - type: volume
        source: elasticsearch
        target: /usr/share/elasticsearch/data
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      ES_JAVA_OPTS: "-Xmx256m -Xms256m"
      ELASTIC_PASSWORD: password
      # Use single node discovery in order to disable production mode and avoid bootstrap checks
      # see https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
      discovery.type: single-node
    networks:
      - elk

  logstash:
    build:
      context: logstash/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./logstash/config/logstash.yml
        target: /usr/share/logstash/config/logstash.yml
        read_only: true
      - type: bind
        source: ./logstash/pipeline
        target: /usr/share/logstash/pipeline
        read_only: true
    ports:
      - "5000:5000/tcp"
      - "5000:5000/udp"
      - "9600:9600"
    environment:
      LS_JAVA_OPTS: "-Xmx256m -Xms256m"
    networks:
      - elk
    depends_on:
      - elasticsearch

  kibana:
    build:
      context: kibana/
      args:
        ELK_VERSION: $ELK_VERSION
    volumes:
      - type: bind
        source: ./kibana/config/kibana.yml
        target: /usr/share/kibana/config/kibana.yml
        read_only: true
    ports:
      - "5601:5601"
    networks:
      - elk
    depends_on:
      - elasticsearch

networks:
  elk:
    driver: bridge

volumes:
  elasticsearch:

```

ELASTIC PASSWORD 부분 변경

```shell
vi docker-stack.yml
```

```yaml
version: '3.3'

services:

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.8.0
    ports:
      - "9200:9200"
      - "9300:9300"
    configs:
      - source: elastic_config
        target: /usr/share/elasticsearch/config/elasticsearch.yml
    environment:
      ES_JAVA_OPTS: "-Xmx256m -Xms256m"
      ELASTIC_PASSWORD: password
      # Use single node discovery in order to disable production mode and avoid bootstrap checks
      # see https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
      discovery.type: single-node
    networks:
      - elk
    deploy:
      mode: replicated
      replicas: 1

  logstash:
    image: docker.elastic.co/logstash/logstash:7.8.0
    ports:
      - "5000:5000"
      - "9600:9600"
    configs:
      - source: logstash_config
        target: /usr/share/logstash/config/logstash.yml
      - source: logstash_pipeline
        target: /usr/share/logstash/pipeline/logstash.conf
    environment:
      LS_JAVA_OPTS: "-Xmx256m -Xms256m"
    networks:
      - elk
    deploy:
      mode: replicated
      replicas: 1

  kibana:
    image: docker.elastic.co/kibana/kibana:7.8.0
    ports:
      - "5601:5601"
    configs:
      - source: kibana_config
        target: /usr/share/kibana/config/kibana.yml
    networks:
      - elk
    deploy:
      mode: replicated
      replicas: 1

configs:

  elastic_config:
    file: ./elasticsearch/config/elasticsearch.yml
  logstash_config:
    file: ./logstash/config/logstash.yml
  logstash_pipeline:
    file: ./logstash/pipeline/logstash.conf
  kibana_config:
    file: ./kibana/config/kibana.yml

networks:
  elk:
    driver: overlay

```

여기도 마찬가지입니다.

### Docker로 실행하기

```shell
docker-compose build && docker-compose up -d
```

### 종료하기

```shell
docker-compose down -v
```

### ELK 포트 및 Kibana 접속하기

기본적으로 ELK에서 사용하는 포트는 다음과 같습니다.

- **Elasticsearch** : 9200 / 9300
- **Logstash** : 5000 / 9600
- **Kibana** : 5601

`http://{ip-address}:5601/` 웹브라우저에서 접속하면 Kibana에 접속할 수 있습니다.

## References

[ELK Docker 설치 방법 - 펜도라](https://judo0179.tistory.com/60)

[도커 + 엘라스틱 서치 Docker로 ElasticSearch ELK 스택 디플로이 - 센의 백과사전 블로그](https://gem1n1.tistory.com/83)

[Elastic Stack 이란? - 17billion github](https://17billion.github.io/elastic/2017/06/30/elastic_stack_overview.html)

[ELK 셋팅부터 알람까지 - 우아한 형제들 기술 블로그](https://woowabros.github.io/experience/2020/01/16/set-elk-with-alarm.html)

