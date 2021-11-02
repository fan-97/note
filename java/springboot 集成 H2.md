# springboot 集成 H2

## 1. 添加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

## 2.添加 数据库配置

`application.properties`

```yaml
spring:
  datasource:
    url: jdbc:h2:file:/data/h2/test # h2 数据库地址
    username: sa  #h2 console username
    password: password # h2 console password
    driverClassName: org.h2.Driver
  jpa:
    spring.jpa.database-platform: org.hibernate.dialect.H2Dialect
```



## 3. 配置h2控制台访问

```yaml
spring:
  h2:
    console.enabled: true
    console.settings.trace: false
    console.path: /h2-console # 访问控制台的地址
    spring.h2.console.settings.web-allow-otheres: false 
```

配置好后，可以通过 `http:localhost/h2-console` 访问