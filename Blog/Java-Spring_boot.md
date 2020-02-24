# [Java] JAVA SPRING BOOT 기초

> 2019.10.26 양재동 코드랩 [JAVA SPRING BOOT 기초 #2](https://www.codelabs.kr/codelabs/detail?no=77)의 정리
> 
> https://github.com/SangHakLee/codelab-spring-boot

- Embeded Tomcat 이용한 단독 실행
- Spring Actuator: 모니터링과 관리


## DB setting
```sql
create database codelab_db default character set utf8;

grant all privileges on codelab_db.* to codelab@localhost identified by 'codelab';

flush privileges;
```

#### 오류
```
You must reset your password using ALTER USER statement before executing this statement.
```
```
mysql> set password = password('PASSWORD');
```

https://kogun82.tistory.com/122

# 실습 1
## IntelliJ

1. Create Project - Spring initializr
2. New Project
3. Dependencies
	4. Dev Tools - `lombok`
	5. WEB - `Spring Web`
	6. SQL - `Spring Data JPA, MySQL Driver`

### DB 연결 정보 셋팅

1. `src` - `main` - `resources`
	1. application.properties 삭제
	1. application.yml 만들기
	```yaml
	spring:
	  datasource:
	    url: jdbc:mysql://127.0.0.1:3306/codelab_db?serverTimezone=Asia/Seoul&useSSL=false&characterEncoding=utf-8
	    username: codelab
	    password: codelab
	    driver-class-name: com.mysql.jdbc.Driver
	  jpa:
	    hibernate:
	      ddl-auto: update # 없으면 만들고 있으면 그대로 사용
	    show-sql: true # mysql debug mode, 실제 쿼리가 로그로 보임
	    database-platform: org.hibernate.dialect.MySQL5InnoDBDialect
	    properties:
	      hibernate: # custom options
	        format_sql: true # sql 로그를 포매팅해서 보여줌
	logging:
	  level:
	    org.hibernate.type: trace # bind 쿼리의 내용까지 출력. ? 부분이 실제 값으로 표현됨
	server:
	  port: 8080
	```

---

- ddl-auto
	- update: 없으면 만들고 있으면 그대로 사용
	- create: 무조건 새로 만듦 


### 기본 컨트롤러  작성

1. `src` - `main` - `java` - `kr.codelabs.member`
	1. package: controller
	1. class: **MyController**
		1. `@RestController`
		1. `@GetMapping`


**RUN!!**

# 실습 2
## DB
1. `src` - `main` - `java` - `kr.codelabs.member`
1. Department
	1. package: entity
	1. class: **Department**
		1. `@Entity`
		1. `@Table`
		1. `@Getter`
1. DepartmentRepository
	1. package: repository
	1. interface: **DepartmentRepository**
1. DepartmentService
	1. package: service
	1. interface: **DepartmentService**
1. DepartmentController
	1. package: controller
	1. class: **DepartmentController**
	
## CRUD
### GET
- `@PathVariable`
- `@RequestParam`

### POST
- `@RequestBody`


## Member





## Swagger

https://mvnrepository.com/artifact/io.springfox/springfox-swagger2

### 1. dependency 추가
`pom.xml`
```xml

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

### config
1. package: config
1. class: SwaggerConfig
1. ApiOperation (각 컨트롤러)

### swagger-ui
http://127.0.0.1:8080/swagger-ui.html


## OAuth 인증 서버 구현

### DB 

```
src - main - resources
	/data.sql
	/schema.sql
```
- [data.sql](https://raw.githubusercontent.com/mac2me/spring-oauth-server/master/OAuthServer/src/main/resources/data.sql)
- [schema.sql](https://raw.githubusercontent.com/mac2me/spring-oauth-server/master/OAuthServer/src/main/resources/schema.sql)

#### application.yml
```yaml
initialization-mode: always # spring 구동 시 resources 하위 .sql 실행
continue-on-error: true # 에러나도 실행. 개발에서만 사용
```

### API 서버
#### pom.xml
```xml
	<dependency>
        <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-oauth2</artifactId>
     </dependency>
     <dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-starter-security</artifactId>
     </dependency>
</dependencies>

<dependencyManagement>
   <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
    </repository>
</repositories>
```
- dependency
	- spring-cloud-starter-oauth2
	- spring-cloud-starter-security
- dependencyManagement
- repositories


## Spring Boot Actuator
https://www.baeldung.com/spring-boot-actuators

- http://localhost:8080/actuator/health
- http://localhost:8080/actuator/env

### jconsole
```bash
$ jconsole
```

# 배포

사전에 maven 설치
```bash
$ mvn package

$ cd target # .jar 파일이 생성됨

$ java -jar NAME.jar
```


zard21@gmail.com
