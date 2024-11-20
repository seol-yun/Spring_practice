# spring 설정

## yml(Mysql)
```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/schoolplanner
    username: yunhwan
    password: 1234
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: create
#      ddl-auto: none
    properties:
      hibernate:
        #        show_sql: true
        format_sql: true
        highlight_sql: true
        dialect: org.hibernate.dialect.MySQLDialect #mysql
    open-in-view: false
```
```
//mysql
	runtimeOnly 'com.mysql:mysql-connector-j'
```

## yml(Oracle)
```
spring:
  datasource:
    url: jdbc:oracle:thin:@172.30.1.1:1521:xe
    username: C##yunhwan
    password: 1234
    driver-class-name: oracle.jdbc.OracleDriver
```

## yml(h2)
```
#server:
#  ssl:
#    key-store: classpath:keystore_test.p12
#    key-store-password: qkqh321@
#    key-store-type: PKCS12
#    key-alias: key_test
#  address: 192.168.56.1
#  port: 8443

spring:
  datasource:
    url: jdbc:h2:tcp://127.0.0.1/~/fitnessdb;
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
#      ddl-auto: create
      ddl-auto: none
    properties:
      hibernate:
        #        show_sql: true
        format_sql: true
        highlight_sql: true
    open-in-view: false

  output:
    ansi:
      enabled: always

#  servlet:
#    encoding:
#      charset : UTF-8
#      force : true
#      enabled : true
#    session:
#      tracking-modes: cookie

logging:
  level:
    org.hibernate.SQL: debug
    org.hibernate.orm.jdbc.bind: trace
#    org.springframework.messaging: trace
#    org.springframework.web.socket: trace


jwt:
  secret: VlwEyVBsYt9V7zq57TejMnVUyzblYcfPQye08f7MGVA9XkHa
  expiration: 86400000  # 24 hours in milliseconds


```


## Gradle
```
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.2'
	id 'io.spring.dependency-management' version '1.1.4'
}

group = 'jpabook'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '17'
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-validation'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-websocket'
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.security:spring-security-crypto'
	testImplementation 'org.springframework.security:spring-security-test'
	implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.2.0'
	implementation 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.junit.jupiter:junit-jupiter-api'
	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'

	implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.5'
	runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.11.5'

	//오라클
	implementation 'org.springframework.boot:spring-boot-starter-jdbc' 
	implementation 'com.oracle.database.jdbc:ojdbc10:19.8.0.0'
}

tasks.named('test') {
	useJUnitPlatform()
}


```



## Github Action
```
name: Deploy to AWS EC2

on:
  push:
    branches:
      - deploy  # deploy 브랜치에 푸시될 때만 실행
      - master

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    # 코드 체크아웃
    - name: Check out the code
      uses: actions/checkout@v3  # 최신 버전으로 업데이트

    # Java 환경 설정
    - name: Set up JDK 17
      uses: actions/setup-java@v3  # 최신 버전으로 업데이트
      with:
        java-version: '17'

    # 프로젝트 빌드
    - name: Build with Gradle
      run: ./gradlew build

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
    - name: Deploy to EC2
      uses: appleboy/ssh-action@v0.1.4
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.EC2_KEY }}
        script: |
          # 프로젝트 파일 전송
          scp -i ${{ secrets.EC2_KEY }} -r ./build/libs/your-app.jar ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }}:/home/ubuntu/

          # EC2에서 실행 중인 이전 프로세스 종료
          pkill -f your-app.jar || true

          # 새 앱 실행
          nohup java -jar /home/ubuntu/your-app.jar > /dev/null 2>&1 &
```
