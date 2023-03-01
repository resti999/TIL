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

# 
