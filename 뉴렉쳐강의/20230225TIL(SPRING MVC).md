# Spring MVC (스프링 웹 MVC) 강의 05 - dispatcher-servlet.xml 파일
* 기존엔 servlet 에 web.xml에 sevlet맵핑이나 어노테이션으로 servlet이란것을 구별했음
* dispatcher 로  spring web mvc library의 org.springframework.web.servlet.DispatcherServlet 클래스를 사용
* web.xml에 설정
```
<servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>dispatcher</servlet-name>
  	<url-pattern>/*</url-pattern>
  </servlet-mapping>
```
* 설정된 front controller(dispathcer)로 모든 url이 거쳐가야한다. front controller가 맵핑해주지 않은 url은 화면에 출력돼선 안된다. -> 그래서 /*로 적어줘야 하는것
* 기본 골격
![image](https://user-images.githubusercontent.com/40667871/221362045-e6108a4e-b8e3-4b39-9aa4-8a5aa6fc8bab.png)
* web-inf 안에 *-servlet.xml 있어야함 *는 web.xml에 설정한 dispathcer 서블릿 이름

# Spring MVC (스프링 웹 MVC) 강의 06 - 스프링 컨트롤러 IndexController 작성하기
* dispatcher-servlet.xml manual 사이트 : https://docs.spring.io/spring-framework/docs/
![image](https://user-images.githubusercontent.com/40667871/221362731-4b88e362-b54b-4c2a-b921-a4dd5aa7d936.png)
* 톰캣 10버전이 되면서 jakartaEE로 바꼈고 Servlet 5.0지원. https://blog.itcode.dev/posts/2022/02/12/tomcat-9-and-10
* spring 동작과정
![image](https://user-images.githubusercontent.com/40667871/221363889-3e6e10af-1fa6-423c-a2df-f8dac5c9bcd3.png)
* dispatcher주소가 /*이면 안되는이유 : forwarding 요청도 dispatcher에 걸리는데 dispatcher엔 그 url이 없어서 -> /로 할경우 모든 url 주소가 dispatcher를 통해 들어오지만 없는 url은 wepapp찾아보게됨



