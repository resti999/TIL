# Spring MVC (스프링 웹 MVC) 강의 15 - Tiles 지시서 작성하기
* tiles 역할
![image](https://user-images.githubusercontent.com/40667871/221578023-4ea545e9-71dd-49ba-bab3-a1b71f2856db.png)
* 우선순위는 tiles 그 다음은 viewResolver
* 요즘은 페이지를 합치는 작업이 프론트에서 이뤄져서 tiles는 사용 안하는 추세
* https://tiles.apache.org/framework/tutorial/basic/pages.html document참조
* tile지시서 작성법
   * controller에 jsp 요청 부분을 notice.list로 변경
   * tiles.xml(WEB-INF 안에) 의 name에 notice.list적기
   * template등 해당 jsp 파일이 있는 경로 써주기
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
  <definition name="notice.list" template="/WEB-INF/view/customer/inc/layout.jsp">
    <put-attribute name="title" value="Tiles tutorial homepage" />
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="body" value="/WEB-INF/view/customer/notice/list.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
  <definition name="notice.detail" template="/WEB-INF/view/customer/inc/layout.jsp">
    <put-attribute name="title" value="Tiles tutorial homepage" />
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="body" value="/WEB-INF/view/customer/notice/detail.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
</tiles-definitions>
```
* 각 부분을 layout어느 부분에 붙이는지?->다음강의에 설명

# Spring MVC (스프링 웹 MVC) 강의 16 - 레이아웃 페이지 만들기와 Tiles 라이브러리 설정하기
![image](https://user-images.githubusercontent.com/40667871/221581647-086a73f2-71a5-44e0-89b3-201b25719454.png)
* 페이지 각 파트 위치 잡는법
* tiles가 제공하는 taglib사용 - taglib사용하기 위해서는 maven에 library추가 필요
* layout.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title><tiles:getAsString name="title"/></title>
    
    <link href="/css/customer/layout.css" type="text/css" rel="stylesheet" />
    <style>
    
        #visual .content-container{	
            height:inherit;
            display:flex; 
            align-items: center;
            
            background: url("../../images/customer/visual.png") no-repeat center;
        }
    </style>
</head>

<body>
    <!-- header 부분 -->

    <tiles:insertAttribute name="header"/>

	<!-- --------------------------- <visual> --------------------------------------- -->
	<!-- visual 부분 -->
	    <tiles:insertAttribute name="visual"/>

	<!-- --------------------------- <body> --------------------------------------- -->
	<div id="body">
		<div class="content-container clearfix">

			<!-- --------------------------- aside --------------------------------------- -->
			<!-- aside 부분 -->
			    <tiles:insertAttribute name="aside"/>

			
			<!-- --------------------------- main --------------------------------------- -->
			    <tiles:insertAttribute name="body"/>


		
		
			
		</div>
	</div>

    <!-- ------------------- <footer> --------------------------------------- -->
	    <tiles:insertAttribute name="footer"/>

       
    </body>
    
    </html>
```
* tiles.xml에서     <put-attribute name="title" value="공지사항" /> 변경.

# Spring MVC (스프링 웹 MVC) 강의 17 - Tiles ViewResolver 설정하기
* dispatcher-servlet.xml 에서 한 것 처럼 notice.list를 연결해주는 tiles viewResolver가 필요하다.
* viewResolver : controller 가 반환하고 있는 값을 jsp에 맵핑 시켜주는 역할
* dispatcher-servlet 변경소스
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
	
	<bean
		class="org.springframework.web.servlet.view.UrlBasedViewResolver">
		<property name="viewClass"
			value="org.springframework.web.servlet.view.tiles3.TilesView" />
		<property name="order" value="1" /> <!-- viewResolver가 2개 이상일 때 우선 순위 설정 -->
	</bean>

	<bean
		class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
		<property name="definitions" value="/WEB-INF/tiles.xml" />
	</bean>
	
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/"></property>
		<property name="suffix" value=".jsp"></property>
		<property name="order" value="2" />
	</bean>
	
	<mvc:resources location="/static/" mapping="/**"></mvc:resources>

</beans>
```
* javax/servlet/jsp/jstl/core/Config library도 있어야만 정상적인 실행이 됨
