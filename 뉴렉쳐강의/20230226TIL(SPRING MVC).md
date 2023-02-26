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
* DetailController 추가
```
package com.newlecture.web.controller.notice;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class DetailController implements Controller{

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		ModelAndView mv = new ModelAndView("notice/detail");
		return mv;
	}

}
```
* dispatcher-servlet.xml 에 설정 bean 추가
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <bean id="/index" class="com.newlecture.web.controller.IndexController"/>  
    <bean id="/notice/list" class="com.newlecture.web.controller.notice.ListConroller"/>  
    <bean id="/notice/detail" class="com.newlecture.web.controller.notice.DetailController"/>  
	
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<mvc:resources location="/static/" mapping="/**"></mvc:resources>

</beans>
```
* jsp 파일에 링크 설정


# Spring MVC (스프링 웹 MVC) 강의 12 - Detail 컨트롤러 추가와 View 페이지 집중화의 필요성
* html 을 controller url로 모두 변경 -> 집중화 필요 -> tiles사용
* maven global repository index rebuild하기
* eclipse maven repository 위치 바꾸는 법: https://mine-it-record.tistory.com/160


# Spring MVC (스프링 웹 MVC) 강의 13 - 페이지 공통분모 집중화
* 페이지들의 공통 분모
![image](https://user-images.githubusercontent.com/40667871/221398853-b4bbb8ab-3dd4-41af-8f12-764bb68da255.png)
* 보통 header, footer는 모든 페이지에 존재 -> 분리
![image](https://user-images.githubusercontent.com/40667871/221398898-8a39ff55-c6ed-4ff6-8b26-2b593e28083b.png)
![image](https://user-images.githubusercontent.com/40667871/221398915-01cdc5c3-6660-4532-8da1-8fd29dcd7a31.png)
![image](https://user-images.githubusercontent.com/40667871/221398946-f19307ac-a875-4e58-9c9e-b5732c9684da.png)
* jsp에서 제공하는 include하는 저 방법의 코드 자체도 귀찮아서 새 방법 찾음-> tiles library

# Spring MVC (스프링 웹 MVC) 강의 14 - 페이지 모듈 분리하기
* header, footer은 모든 페이지 공통 -> root 밑의 inc 폴더안에 보관
* aside, visual, layout은 cutomer페이지의 공통 -> root밑의 cutomer밑의 inc 안에 보관 / notice폴더도 customer안으로 이동
* notice안의 list.jsp, detail.jsp 는 각각 main부분만 남겨놓고 공통된 부분은 inc에 보관
* tiles library를 이용해 합칠 예정(코드 집중화)
