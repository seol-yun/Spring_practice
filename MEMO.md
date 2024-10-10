# MEMO 

## 스프링 프로젝트 생성

3.2.x

스프링 부트 스타터(https://start.spring.io/)

Project: **Gradle - Groovy** Project

사용 기능: web, thymeleaf, jpa, h2, lombok, validation 등등

groupId: jpabook

artifactId: jpashop

스프링부트 3.0.x 버전은 그래들 자바 17아닌거 하면 충돌남


## h2
### h2 파일 생성:

`jdbc:h2:~/jpashop` (최소 한번)

`~/jpashop.mv.db` 파일 생성 확인

이후 부터는 `jdbc:h2:tcp://localhost/~/jpashop` 이렇게 접속

### h2 실행: 

h2에서 wsl은 ./h2.sh, cmd는 h2.bat


## gradle 빌드
setting에서 빌드도구 gradle로 설정 후 실행하면 안텔리제이에서 도구 사용 가능 


## 스웨거
spring 3버전 이상부터는 스웨거 사용시  springdoc-openapi-ui 사용하기

implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.2.0'

http://localhost:8080/swagger-ui/index.html로 접속

## 기타 팁
fetch할때 json인지 text인지 명확하게 전달하기


경로설정은 항상 application.properties에서

Error parsing HTTP request header Note: further occurrences of HTTP request parsing errors will be logged at DEBUG level.
분명 http로 접속했는데 계속해서 생기는 오류 => 자바스크립트에서 https경로를 사용하였다. 주석처리되어있어도 안됨! 주석에 주의하자

localhost가 아닌 ip 변경시 https로 접속 -> ssl 인증받아야함

cmd에서 
keytool -genkey -alias key_test -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore_test.p12 -validity 4000

server.address=192.168.0.21
server.port=8443
server.ssl.key-store= classpath:keystore_test.p12
server.ssl.key-store-password=비번
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=key_test

무조건 절대경로만 되는것같다.. 정정) classpath:keystore_test.p12 클래스패스로 가능 클래스패스는 application.properties의 경로


jar파일 생성후 docker에 띄우기


gradlew build 안될때 = 배포를 웨해 jar파일을 받아야하는데 빌드가 자꾸 안됐따.

1. 경로 똑같이 맞추기1 (MyCustomTest 클래스를 com.demo.demo 로 이동)

2. 경로 똑같이 맞추기2 (com.demo.others 에도 스프링부트 설정 패키지 생성)

3. 상위 경로에 설정 클래스 하나 두기 (com 또는 com.demo 에 에러 방지용 디폴트 설정 클래스 생성)

어쨌든 테스트 실행 클래스가 찾을 수 있는 설정 파일이 있기만 하면 된다.

마지막으로 \build\libs>java -jar *.jar

SNAPSHOT.jar는 실행가능한 아카이브이고, SNAPSHOT-plain.jar는 실행이 불가능한 일반 아카이브이다.

스프링부트 2.5부터 빌드시 기본적으로 2가지 파일이 떨어지게 되었는데, 빌드시 plain.jar를 생성하지 않으려면 build.gradle에 아래 내용을 추가해주면 된다.

  jar {
      enabled = false
  }




도커파일 생성하고 빌드하면 도커가 자동으로 이미지를 빌드함

//[Dockerfile]
FROM openjdk:17
ARG JAR_FILE=build/libs/hello-0.0.1-SNAPSHOT.jar
COPY ${JAR_FILE} app.jar

ARG KEYSTORE_FILE=src/main/resources/keystore_test.p12
COPY ${KEYSTORE_FILE} /keystore_test.p12

ENTRYPOINT ["java","-jar","/app.jar"]


상대경로로 해야한다.

docker build -t (레포지토리이름):(테그) .

docker run --name springHello -p 8443:8443 spring:hello



그래들 빌드 -> 도커 빌드 -> 도커 실행


wsl에서는 window ip와 다르니 주의하자... 하... 리눅스에서 ifconfig로 ip찾아서 테스트하기

build.gradle 우클릭 후 import gradle
