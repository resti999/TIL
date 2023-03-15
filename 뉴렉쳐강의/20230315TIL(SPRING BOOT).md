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
* admin도 나눈 후 controller 에서 tiles 형식으로 "admin.board.notice.list"로 반환
* index, help page 도 tiles 설정하기
* root에 대한 tile설정을 할 때 "*"로 tiles  name을 설정하면 모든 url에 대해서 동작하기 때문에  가상으로 "home.*"라고 쓰고 Controller에도 반영(tiles name은 약속만 지키면되기 때문, 실제 경로 안지켜도 됨). 
* HomeController 변경
```
package com.newlecture.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {
	
	
	
	
	
	@RequestMapping("/index")
	public String asdf() {
		return "home.index";
	
	}
	
	
	@RequestMapping("/help")
	public String aaa() {
		return "home.help";
	}
}

```
* tiles.xml
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
  <definition name="home.*" template="/WEB-INF/view/inc/layout.jsp">
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/{1}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
</tiles-definitions>
```

# Sping Boot 2.x Quick Start 강의 18 - Tiles의 추가 기능들
* tiles 의 기능 중 상속을 통한 집중화 , 동일 한 부분은 넘겨받음, 
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
  <definition name="layout.common" template="/WEB-INF/view/inc/layout.jsp">
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/{1}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
    <definition name="home.*"  extends="layout.common">
    <put-attribute name="main" value="/WEB-INF/view/{1}.jsp" />
  </definition>
  <definition name="customer.*.*" template="/WEB-INF/view/customer/inc/layout.jsp" extends="layout.common">
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/customer/{1}/{2}.jsp" />
  </definition>
  <definition name="admin.*.*.*" template="/WEB-INF/view/admin/inc/layout.jsp" extends="layout.common">
    <put-attribute name="visual" value="/WEB-INF/view/admin/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/admin/inc/aside.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/admin/{1}/{2}/{3}.jsp" />
  </definition>
</tiles-definitions>
```

# Sping Boot 2.x Quick Start 강의 19 - MyBatis 설정하는 방법
![image](https://user-images.githubusercontent.com/40667871/225341144-40bed863-ff59-4b76-bc73-6b40822f691b.png)
* service layer 에선 sql을 몰라도 됨, 데이터 소스 관심 x가능
* DAO를 구현할 때 반복적인 부분을 줄여주는 역할이 mybatis lib
* mybatis 사용을 위해 해야할 것들
   * mybatis lib 다운 -> maven
   ![image](https://user-images.githubusercontent.com/40667871/225343475-89c66663-7428-436d-a8d1-40e82a563a70.png)
   * mysql lib 다운 -> maven
   ![image](https://user-images.githubusercontent.com/40667871/225343431-a14a964b-ee42-4f26-b3f9-a7b66b52f052.png)

   ![image](https://user-images.githubusercontent.com/40667871/225342195-94cc0ff1-4774-4a42-99a8-eff59c6284c6.png)
   * datasource설정(db 주소 id, pw등) - application.properties에서
   ![image](https://user-images.githubusercontent.com/40667871/225343124-0f6633ee-e8be-469a-bec8-5da79afb41ad.png)
* mybatis 사용
![image](https://user-images.githubusercontent.com/40667871/225343170-f5516b55-6888-495a-ad95-c4dae76f98be.png)

#
