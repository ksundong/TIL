# Docker 호스트와 컨테이너 간 파일 전송하기

Docker 호스트(컨테이너)에서 컨테이너(호스트)로 파일을 복사하는 명령어인 `docker cp`를 사용한다.

- 호스트에서 컨테이너로 파일을 복사하는 경우

```shell
docker cp [OPTIONS] SRC_PATH CONTAINER_[NAME|ID]:DEST_PATH
```

- 컨테이너에서 호스트로 파일을 복사하는 경우

```shell
docker cp [OPTIONS] CONTAINER_[NAME|ID]:SRC_PATH DEST_PATH
```
