# Spring MVC (스프링 웹 MVC) 강의 18 - Tiles 설정에 Wildcard 이용하기

*

# Spring MVC (스프링 웹 MVC) 강의 19 - Root 페이지들을 위한 Layout 페이지 만들기

* tilse.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
  <definition name="root.*" template="/WEB-INF/view/inc/layout.jsp">
    <put-attribute name="title" value="공지사항" />
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="body" value="/WEB-INF/view/{1}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
  <definition name="notice.*" template="/WEB-INF/view/customer/inc/layout.jsp">
    <put-attribute name="title" value="공지사항" />
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="body" value="/WEB-INF/view/customer/notice/{1}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
</tiles-definitions>
```

* IndexConroller
```
package com.newlecture.web.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class IndexController implements Controller {

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		
		ModelAndView mv = new ModelAndView("root.index");
		mv.addObject("data", "Hello Spring MVC~");
//		mv.setViewName("/WEB-INF/view/index.jsp");
		return mv;
	}


	
}

```

* root layout.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<!DOCTYPE html>
<html>

<head>
    <title>코딩 전문가를 만들기 위한 온라인 강의 시스템</title>
    <meta charset="UTF-8">
    <title>공지사항목록</title>

    <link href="/css/layout.css" type="text/css" rel="stylesheet" />
    <link href="/css/index.css" type="text/css" rel="stylesheet" />
    <script>
    
    </script>
</head>

<body>
    <!-- header 부분 -->

    <tiles:insertAttribute name="header"/>


    <!-- --------------------------- <body> --------------------------------------- -->

    
    <tiles:insertAttribute name="body"/>



    <!-- ------------------- <footer> --------------------------------------- -->

    <tiles:insertAttribute name="footer"/>

    
</body>

</html>
```

# Spring MVC (스프링 웹 MVC) 강의 20 - 데이터 서비스 클래스(NoticeService) 준비하기
* jdbc강의에서 쓰던 NoticeService, Notice 복붙

# Spring MVC (스프링 웹 MVC) 강의 21 - 서비스 객체 사용하기
* db연동을 위해 ojdbc 19.7버전 maven으로 dependency설정
* NoticeService bean으로 객체 생성(IoC Container에)
* ![image](https://user-images.githubusercontent.com/40667871/222128488-7a8ce573-dd31-4fd8-b409-f2afc3087fa2.png)
* IoC Container의 객체 사용법
* el tag의 n.id = getterId
* dispatcher-servlet.xml 변경사항 : noticeService bean생성 후 listController 의 프로퍼티로 집어넣기
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
    <bean id="/notice/list" class="com.newlecture.web.controller.notice.ListConroller">
    	<property name="noticeService" ref="noticeService"/> <!--name 은 ListController의 setter  -->
    </bean>  
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
	
	<bean id="noticeService" class="com.newlecture.web.service.NoticeService"/>

</beans>
```
* ListController에 NoticeService 멤버변수, setter 만들기
```
package com.newlecture.web.controller.notice;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

public class ListConroller implements Controller{
	
	
	private NoticeService noticeService;
	
	public void setNoticeService(NoticeService noticeService) {
		this.noticeService = noticeService;
	}

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		
		
		ModelAndView mv = new ModelAndView("notice.list");
//		mv.setViewName("/WEB-INF/view/index.jsp");
		
		List<Notice> list = noticeService.getList(1, "TITLE", "");
		mv.addObject("list",list);
		
		return mv;
	}

}

```
* notice_view 아래와 같음
```
SELECT * FROM (
    SELECT ROWNUM NUM, N.* FROM (
        SELECT * FROM NOTICE ORDER BY REGDATE DESC, ID DESC
    ) N
) N2
```
* customer/notice/list.jsp
```
<c:forEach var="n" items="${list}">		
					<tr>
						<td>${n.id}</td>
						<td class="title indent text-align-left"><a href="detail">${n.title}</a></td>
						<td>${n.writerId}</td>
						<td>
							${n.regDate}	
						</td>
						<td>${n.hit}</td>
					</tr>
					</c:forEach>	
```

# Spring MVC (스프링 웹 MVC) 강의 22 - 서비스 객체 분리하기
![image](https://user-images.githubusercontent.com/40667871/222140134-5d131915-329b-4ac2-b214-2d1d984fad50.png)

* 서비스 구현할 구현기술이 달라질 경우( jdbc가 아닌 jpa, mybatis등) , 현재는 jdbc객체를 콕 짚어서 멤버변수로 선언해놔서 결합력이 굉장히 높음(jdbc로 구현된 NoticeService와)  ->해결법: 인터페이스 이용
* 보통은 Service class에서 직접 db의 정보에 접근하진 않고 DAO라는 한가지의 계층을 더 만들고 Service에선 그걸 갖다 쓰는 방식
* NoticeService Interface생성
```
package com.newlecture.web.service;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import com.newlecture.web.entity.Notice;

public interface NoticeService {

	List<Notice> getList(int page, String field, String query) throws ClassNotFoundException, SQLException;
	
	int getCount() throws ClassNotFoundException, SQLException;

	int insert(Notice notice) throws SQLException, ClassNotFoundException;
	
	int update(Notice notice) throws SQLException, ClassNotFoundException;
	
	int delete(int id) throws ClassNotFoundException, SQLException;

}

```
* 기존 NoticeService -> JDBCNoticeService로 이름 변경
* ListController에 데이터자료형 인터페이스로
```
package com.newlecture.web.controller.notice;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;
import com.newlecture.web.service.jdbc.JDBCNoticeService;

public class ListConroller implements Controller{
	
	
	private NoticeService noticeService;
	
	public void setNoticeService(NoticeService noticeService) {
		this.noticeService = noticeService;
	}

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		
		
		ModelAndView mv = new ModelAndView("notice.list");
//		mv.setViewName("/WEB-INF/view/index.jsp");
		
		List<Notice> list = noticeService.getList(1, "TITLE", "");
		mv.addObject("list",list);
		
		return mv;
	}

}
```
* dispatcher-servlet.xml 변경 : NoticeService(기존이름)이었떤 bean의 이름을 JDBCNoticeService로 이름 변경, 패키지명도 신경쓰기
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
    <bean id="/notice/list" class="com.newlecture.web.controller.notice.ListConroller">
    	<property name="noticeService" ref="noticeService"/> <!--name 은 ListController의 setter  -->
    </bean>  
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
	
	<bean id="noticeService" class="com.newlecture.web.service.jdbc.JDBCNoticeService"/>

</beans>
```
