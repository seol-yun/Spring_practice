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
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create-drop
    properties:
      hibernate:
#        show_sql: true
        format_sql: true
        highlight_sql: true
    open-in-view: false

  output:
    ansi:
      enabled: always

logging:
  level:
    org.hibernate.SQL: debug
    org.hibernate.orm.jdbc.bind: trace

```
