# spring-session

Spring Security and Angular
https://spring.io/guides/tutorials/spring-security-and-angular-js/
A tutorial on how to use Spring Security with a single page application with various backend architectures, ranging from a simple single server to an API gateway with OAuth2 authentication.

一个关于如何使用Spring Security与一个具有不同后端架构的单页应用程序的教程，从简单的单服务器到具有OAuth2认证的API网关。

spring-session

CMD运行程序命令：

1、mvn spring-boot:run
2、mvn package
3、cd target
4、java -jar spring-session-ui-0.0.1-SNAPSHOT.jar

STS运行方式:

1、Run as -> Maven clean
2、Run as -> Maven install
3、UiApplication.java Java Application

注意：
1、修改/spring-session-ui/pom.xml文件
UiApplication.java文件在STS中以Java Application方式运行时，
生成的\ui\target\classes\static文件夹又被删掉了，
原因不明，但注释掉以下代码，可以解决此问题：
<!-- 					<execution> -->
<!-- 						<id>npm-test</id> -->
<!-- 						<goals> -->
<!-- 							<goal>npm</goal> -->
<!-- 						</goals> -->
<!-- 						<configuration> -->
<!-- 							<arguments>run-script e2e</arguments> -->
<!-- 						</configuration> -->
<!-- 						<phase>test</phase> -->
<!-- 					</execution> -->

http://localhost:8080/home
http://localhost:8080/login
user/password

修改代码，支持POST等方式：

1、修改/spring-session-ui/src/app/home.component.ts
增加
const token = data['token'];
localStorage.setItem('token', token);
	  
把http.get('http://localhost:9000', {headers : new HttpHeaders().set('X-Auth-Token', token)})
改为：http.post('http://localhost:9000', {headers : new HttpHeaders().set('X-Auth-Token', token)})

2、增加/spring-session-ui/src/app/authentication.interceptor.ts文件

3、修改/spring-session-ui/src/app/app.module.ts
增加import {AuthenticationInterceptor} from './authentication.interceptor';
增加{ provide: HTTP_INTERCEPTORS, useClass: XhrInterceptor, multi: true }, [{provide: HTTP_INTERCEPTORS, useClass: AuthenticationInterceptor, multi: true}]
到providers中

4、修改/spring-session-resource/src/main/java/demo/ResourceApplication.java
把@RequestMapping("/")
改为：@PostMapping("/")
