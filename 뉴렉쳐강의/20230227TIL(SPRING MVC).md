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
