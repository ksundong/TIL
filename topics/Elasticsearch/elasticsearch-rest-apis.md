# Elasticsearch REST API 들

이 글은 Elasticsearch 7.x 이상부터 적용되고, 실제 테스트는 7.8버전에서 이루어졌습니다.

GET 메서드로 Elasticsearch 클러스터 상태 정보 조회

```shell
curl -XGET "http://localhost:9200"
```

## CRUD(입력, 조회, 수정, 삭제)

### PUT(수정 및 입력)

```shell
curl -XPUT "http://localhost:9200/my_index/_doc/1" -H 'Content-Type: application/json' -d'
{
  "book": "함께자라기",
  "tag": "애자일"
}'
```

#### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "1",
  "_version": 1,
  "result": "created",
  "_shards": { "total": 2, "successful": 1, "failed": 0 },
  "_seq_no": 0,
  "_primary_term": 1
}
```

응답의 `result` key의 value가 `created` 라면 생성이 된 것 입니다.

하지만 PUT이어서 덮어씌워질 우려가 있습니다. 그렇게 되면 result의 값은 `updated`가 됩니다.

#### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "1",
  "_version": 2,
  "result": "updated",
  "_shards": { "total": 2, "successful": 1, "failed": 0 },
  "_seq_no": 1,
  "_primary_term": 1
}
```

이를 방지하기 위해 `_doc`을 `_create`로 변경합니다.

```shell
curl -XPUT "http://localhost:9200/my_index/_create/1" -H 'Content-Type: application/json' -d'
{
  "book": "함께자라기",
  "tag": "애자일"
}'
```

#### 응답

```json
{
  "error": {
    "root_cause": [
      {
        "type": "version_conflict_engine_exception",
        "reason": "[1]: version conflict, document already exists (current version [1])",
        "index_uuid": "_TWtgW4sTCSinY2RTNncGQ",
        "shard": "0",
        "index": "my_index"
      }
    ],
    "type": "version_conflict_engine_exception",
    "reason": "[1]: version conflict, document already exists (current version [1])",
    "index_uuid": "_TWtgW4sTCSinY2RTNncGQ",
    "shard": "0",
    "index": "my_index"
  },
  "status": 409
}
```

이미 존재하기 때문에 409 스테이터스 코드(Conflict)를 줍니다. 아마 status key로 409를 주는 이유는 안티패턴이지만 legacy 때문이 아닐까 조심스레 추측해봅니다.

### GET(조회)

```shell
curl -XGET "http://localhost:9200/my_index/_doc/1" -H 'Content-Type: application/json'
```

#### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "1",
  "_version": 2,
  "_seq_no": 1,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "book": "함께자라기",
    "tag": "애자일"
  }
}
```

우리가 입력한 데이터는 `_source`에 있는 것을 확인할 수 있습니다.

### DELETE(삭제)

DELETE를 사용하면 도큐먼트 또는 인덱스 단위의 삭제가 가능합니다.

```shell
curl -XDELETE "http://localhost:9200/my_index/_doc/1" -H 'Content-Type: application/json'
```

#### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "1",
  "_version": 3,
  "result": "deleted",
  "_shards": { "total": 2, "successful": 1, "failed": 0 },
  "_seq_no": 2,
  "_primary_term": 1
}
```

result가 `deleted`로 반환됩니다.

이 때 조회를 해봅시다.

```shell
curl -XGET "http://localhost:9200/my_index/_doc/1" -H 'Content-Type: application/json'
```

#### 응답

```json
{ "_index": "my_index", "_type": "_doc", "_id": "1", "found": false }
```

`"found": false`라는 응답을 줍니다.

인덱스를 삭제해봅시다.

```shell
curl -XDELETE "http://localhost:9200/my_index" -H 'Content-Type: application/json'
```

#### 응답

```json
{ "acknowledged": true }
```

심플한 응답을 줍니다.

다시 도큐먼트를 조회하면 다른 에러가 나옵니다.

```shell
curl -XGET "http://localhost:9200/my_index/_doc/1" -H 'Content-Type: application/json'
```

#### 응답

```json
{
  "error": {
    "root_cause": [
      {
        "type": "index_not_found_exception",
        "reason": "no such index [my_index]",
        "resource.type": "index_expression",
        "resource.id": "my_index",
        "index_uuid": "_na_",
        "index": "my_index"
      }
    ],
    "type": "index_not_found_exception",
    "reason": "no such index [my_index]",
    "resource.type": "index_expression",
    "resource.id": "my_index",
    "index_uuid": "_na_",
    "index": "my_index"
  },
  "status": 404
}
```

`"type":"index_not_found_exception"` 이 발생합니다.

### 입력(POST) 및 수정(Update API)

#### 입력 API 사용

POST는 id를 지정하지 않고, 데이터를 입력하는데 사용할 수 있습니다.

```shell
curl -XPOST "http://localhost:9200/my_index/_doc" -H 'Content-Type: application/json' -d'
{
  "book": "함께자라기",
  "tag": "애자일"
}'
```

##### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "oWSFvHMBPvgyY3MVXs8O",
  "_version": 1,
  "result": "created",
  "_shards": { "total": 2, "successful": 1, "failed": 0 },
  "_seq_no": 0,
  "_primary_term": 1
}
```

document의 id가 `"_id": "oWSFvHMBPvgyY3MVXs8O"`로 자동생성 되었음을 알 수 있습니다.

#### 수정 API 사용

PUT을 사용하려면 전체 정보를 다 알고 있어야합니다. 부분 수정을 하기 위해서 수정 API를 사용합니다.

수정 API에서는 스크립트도 사용가능하고, 문서의 부분을 수정하는 것도 가능합니다.

자세한 내용은 [공식 문서](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html)를 참고해주세요.

```shell
curl -XPOST "http://localhost:9200/my_index/_update/oWSFvHMBPvgyY3MVXs8O" -H 'Content-Type: application/json' -d'
{
  "doc": {
    "book": "함께 자라기"
  }
}'
```

##### 응답

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "oWSFvHMBPvgyY3MVXs8O",
  "_version": 2,
  "result": "updated",
  "_shards": { "total": 2, "successful": 1, "failed": 0 },
  "_seq_no": 1,
  "_primary_term": 1
}
```

변경될 내용이 없는 경우

```json
{
  "_index": "my_index",
  "_type": "_doc",
  "_id": "oWSFvHMBPvgyY3MVXs8O",
  "_version": 2,
  "result": "noop",
  "_shards": { "total": 0, "successful": 0, "failed": 0 },
  "_seq_no": 1,
  "_primary_term": 1
}
```

둘의 차이는 result의 차이입니다.

업데이트 후 조회를 해보면 `"_version": 2`로 표시되어 있습니다. 이는 Elasticsearch의 주요한 특징 중 하나로, 데이터는 불변하며, 전체 내용을 갱신하게 됩니다.

## References

[CRUD - 입력, 조회, 수정, 삭제 - Elastic 가이드북](https://esbook.kimjmin.net/04-data/4.2-crud)
