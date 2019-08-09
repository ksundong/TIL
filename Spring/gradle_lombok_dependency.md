# IntelliJ IDEA Gradle Lombok Dependency Setting

IntelliJ로  Spring Project를 진행하려고 하는데, Lombok으로 설정된 symbol을 찾을 수 없다는 오류가 발생했다.

Google에서 열심히 찾아보았으나, Annotation Processor는 IntelliJ에서 이미 Lombok을 쓰면 활성화 하라고 알림을 친절하게 띄워주어서 활성화 된 상태였고, Lombok 플러그인도 Setting 된 상태였다.

Test를 돌릴 때 에러가 발생하는 것 같아, test에도 Dependency를 작성하였다.

``` gradle
    //LOMBOK Dependencies
    compileOnly 'org.projectlombok:lombok:1.18.8'
    annotationProcessor 'org.projectlombok:lombok:1.18.8'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.8'
    testCompile 'org.projectlombok:lombok:1.18.8'
    testImplementation 'org.projectlombok:lombok:1.18.8'
```

