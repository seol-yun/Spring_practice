# spring 설정


## properties(Oracle)
```
server.address=localhost
server.port=8081
spring.datasource.url=jdbc:oracle:thin:@localhost:1521:xe
spring.datasource.username=C##yun
spring.datasource.password=1234
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
##jpa
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.highlight_sql=true
spring.jpa.hibernate.ddl-auto=none

logging.level.org.hibernate.SQL=debug
logging.level.org.hibernate.orm.jdbc.bind = trace
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
//	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
//	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
//	implementation 'org.springframework.boot:spring-boot-starter-web'
//	compileOnly 'org.projectlombok:lombok'
//	annotationProcessor 'org.projectlombok:lombok'
//	testImplementation 'org.springframework.boot:spring-boot-starter-test'
//	implementation 'com.oracle.database.jdbc:ojdbc10:19.8.0.0'


//	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
//	implementation 'org.springframework.boot:spring-boot-starter-web'
//	implementation 'org.springframework.boot:spring-boot-devtools'
//	testImplementation ('org.springframework.boot:spring-boot-starter-test'){
//		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
//	}
//	implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.0.2'
//	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
////	implementation 'com.oracle.database.jdbc:ojdbc10:19.8.0.0'
//	runtimeOnly 'com.h2database:h2'
//	testImplementation 'junit:junit:4.13.1'
//	compileOnly 'org.projectlombok:lombok'
//	annotationProcessor 'org.projectlombok:lombok'
//	implementation 'org.springframework.boot:spring-boot-starter-validation'
//	//배포에서는 빼기
//	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'

	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-validation'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
//JUnit4 추가
	testImplementation("org.junit.vintage:junit-vintage-engine") {
		exclude group: "org.hamcrest", module: "hamcrest-core"
	}

	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'
}

tasks.named('test') {
	useJUnitPlatform()
}

```
