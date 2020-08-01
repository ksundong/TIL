# IntelliJ IDEA Plain Java 프로젝트를 Gradle 프로젝트로 바꾸는 방법

Gradle은 설치되어 있다고 가정하였습니다. 그리고 `.gitignore` 파일을 백업해두시길 바랍니다.

먼저 터미널에서 gradle init으로 gradle 프로젝트를 설정합니다.

application으로 생성해야 추후 작업이 간단해집니다.

그리고 나서 `AppTest.java` `App.java`순으로 제거해줍니다.

생성된 `build.gradle`파일에 아래와 같이 입력합니다.

```groovy
id 'java'
id 'idea' // 추가
```

그 다음 하단 터미널에 아래와 같이 입력합니다.

```shell
gradle cleanIdea
gradle idea
```

기존 패키지들은 main에 넣어줍니다.

그리고 나서 File에 Invalidate Caches / Restart를 눌러 다시 시작해줍니다.

재시작 하고 나면 Gradle 프로젝트를 Import하겠냐는 창이 우측에 뜨는데 Import를 해줍니다.

그러면 우측에 Gradle 창이 열릴 것 입니다.

그리고 나서 build.gradle 파일에 들어가면 상단에 뜨는 것은 Apply 해주면 됩니다.

자동으로 `.gitignore`를 생성해주므로 `.gitignore` 파일에 기존의 `.gitignore` 파일의 내용과 아래의 내용을 추가해주면 됩니다.

```
# Avoid ignoring Gradle wrapper jar file (.jar files are usually ignored)
!gradle-wrapper.jar
```

