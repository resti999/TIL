# Spring MVC (스프링 웹 MVC) 강의 07 - View 페이지를 위한 위치
* Dispatcher-servlet.xml에서 /*->/로 바꿨을 때의 문제점
* Controller 를 건너뛰고 jsp를 요청하면 안된다. 원래 Controller 와 View는 한 몸둥이였는데 유지보수를 위해 분리한 것 뿐 -> 해결법: WEB-INF에 jsp파일 넣기. 원격에서 WEB-INF접근은 안되나 서버에서  forward를 통한 접근은 가능하기 때문
* 상대주소로view를 찾을 때 url이 conroller mapping url에 영향을 받는다. controller mapping url 과 같은 위치에 있다고 보기때문 ->그래서 절대경로로 view를 찾는것이 낫다

# Spring MVC (스프링 웹 MVC) 강의 08 - ViewResolver 사용하기
![image](https://user-images.githubusercontent.com/40667871/221393280-0257e1c8-b32f-4afd-98e3-68da9c22c80e.png)
* setViewName이 아닌 오버로드생성자로도 jsp경로 설정 가능
* 중복되는 WEB-INF/view 정보 담는 ViewResolver
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="/index" class="com.newlecture.web.controller.IndexController">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>
	
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>

</beans>
```

# Spring MVC (스프링 웹 MVC) 강의 09 - HTML 파일 설정하기
* 경로는 맞는데 image 출력이 안되는 이슈 발생


# Spring MVC (스프링 웹 MVC) 강의 10 - 정적파일 서비스하기
* 기본적으로 정적인 파일을 spring이 제공하지 않도록 하고 있기 때문 html, 이미지는 불가 jsp는 가능
* /* ->jsp도 막겠다.   / -> jsp 뺀 정적파일들을 막겠다
* 정적파일 허용 코드
![image](https://user-images.githubusercontent.com/40667871/221395052-18b14903-cd3f-4cae-a8a2-7931d0a69523.png)
* 스키마 파일 - dispatcher-servlet파일 안에서 사용할 수 있는 태그 집합을 갖고 있는것
* 스키마 파일 : https://www.springframework.org/schema/mvc/spring-mvc.xsd
* 스키마 파일에 대한 정의 : http://www.springframework.org/schema/mvc
* 스키마 파일 정의 변수화 : xmlns:mvc="http://www.springframework.org/schema/mvc
* 정적파일 허용하는 dispatcher-servlet.xml설정
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <bean id="/index" class="com.newlecture.web.controller.IndexController">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>
	
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<mvc:resources location="/resource/" mapping="/resource/**"></mvc:resources>

</beans>
```
* 	<mvc:resources location="/static/" mapping="/**"></mvc:resources> 맵핑의 의미 : /을 루트로 오는 정적파일url은 실제 /static 이 루트인 것처럼 탐색

# Spring MVC (스프링 웹 MVC) 강의 11 - 공지사항 컨트롤러 추가하기
*

# 12
# 13
# 14
