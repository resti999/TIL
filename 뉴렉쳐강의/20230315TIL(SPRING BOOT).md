# Sping Boot 2.x Quick Start 강의 16 - Tiles 패턴 사용하기
* * 로 패턴을 듦 
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
  <definition name="customer.*.*" template="/WEB-INF/view/customer/inc/layout.jsp">
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/customer/{1}/{2}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
  <definition name="admin.*.*.*" template="/WEB-INF/view/admin/inc/layout.jsp">
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/admin/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/admin/inc/aside.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/admin/{1}/{2}/{3}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
</tiles-definitions>
```

# Sping Boot 2.x Quick Start 강의 17 - Admin을 위한 Layout 페이지 만들기
* admin layout.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles"  %>   
<!DOCTYPE html>
<html>

<head>
	<title>코딩 전문가를 만들기 위한 온라인 강의 시스템</title>
	<meta charset="UTF-8">
	<title>공지사항목록</title>

	<link href="/css/customer/layout.css" type="text/css" rel="stylesheet" />
	<style>
		#visual .content-container {
			height: inherit;
			display: flex;
			align-items: center;

			background: url("/images/mypage/visual.png") no-repeat center;
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
				<tiles:insertAttribute name="main"/>
		</div>
	</div>

	<!-- ------------------- <footer> --------------------------------------- -->

		<tiles:insertAttribute name="footer"/>

	
</body>

</html>
```
