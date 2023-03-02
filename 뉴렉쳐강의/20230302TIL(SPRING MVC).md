# Spring MVC (스프링 웹 MVC) 강의 23 - 연결정보 분리하기
* JDBCNoticeService의 db커넥정보를 코드로 입력했는데, 나중에 배포 후 바이너리 코드일 때 db비밀번호가 바뀌면 비밀번호를 변경하기 곤란해진다.
* 커넥션 정보 spring xml파일로 변경
* JDBCNoticeService : 커넥션  정보 지운 후 datasource인터페이스 생성 후   커넥션 때 사용
```
package com.newlecture.web.service.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.sql.DataSource;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

public class JDBCNoticeService implements NoticeService {
//	private String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
//	private String uid = "NEWLEC";
//	private String pwd = "24rkwk";
//	private String driver = "oracle.jdbc.driver.OracleDriver";
	
	private DataSource dataSource;
	
	public void setDataSource(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public List<Notice> getList(int page, String field, String query) throws ClassNotFoundException, SQLException{
		
		int start = 1 + (page-1)*10;     // 1, 11, 21, 31, ..
		int end = 10*page; // 10, 20, 30, 40...
		
		String sql = "SELECT * FROM NOTICE_VIEW WHERE "+field+" LIKE ? AND NUM BETWEEN ? AND ?";	
//		String sql = "select * from "
//				+ "("
//				+ "    select rownum num ,id, title, writer_id, regdate, hit, files, pub,cmt_count "
//				+ "    from "
//				+ "    ("
//				+ "        select * from notice_view order by regdate desc, id desc"
//				+ "    ) main"
//				+ ") main "
//				+ "where "+field+" like ? and num between ? and ?";
		
		
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, "%"+query+"%");
		st.setInt(2, start);
		st.setInt(3, end);
		ResultSet rs = st.executeQuery();
		
		List<Notice> list = new ArrayList<Notice>();
		
		while(rs.next()){
		    int id = rs.getInt("ID");
		    String title = rs.getString("TITLE");
		    String writerId = rs.getString("WRITER_ID");
		    Date regDate = rs.getDate("REGDATE");
		    String content = rs.getString("CONTENT");
		    int hit = rs.getInt("hit");
		    String files = rs.getString("FILES");
		    
		    Notice notice = new Notice(
		    					id,
		    					title,
		    					writerId,
		    					regDate,
		    					content,
		    					hit,
		    					files
		    				);

		    list.add(notice);
		    
		}

		
		rs.close();
		st.close();
		con.close();
		
		return list;
	}
	
	// Scalar 
	public int getCount() throws ClassNotFoundException, SQLException {
		int count = 0;
		
		String sql = "SELECT COUNT(ID) COUNT FROM NOTICE";	
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		Statement st = con.createStatement();
		
		ResultSet rs = st.executeQuery(sql);
		
		if(rs.next())
			count = rs.getInt("COUNT");		
		
		rs.close();
		st.close();
		con.close();
		
		return count;
	}

	public int insert(Notice notice) throws SQLException, ClassNotFoundException {
		String title = notice.getTitle();
		String writerId = notice.getWriterId();
		String content = notice.getContent();
		String files = notice.getFiles();
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "INSERT INTO notice (    " + 
				"    title," + 
				"    writer_id," + 
				"    content," + 
				"    files" + 
				") VALUES (?,?,?,?)";	
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, title);
		st.setString(2, writerId);
		st.setString(3, content);
		st.setString(4, files);
		
		int result = st.executeUpdate();
		
		
		st.close();
		con.close();
		
		return result;
	}
	
	public int update(Notice notice) throws SQLException, ClassNotFoundException {
		String title = notice.getTitle();
		String content = notice.getContent();
		String files = notice.getFiles();
		int id = notice.getId();
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "UPDATE NOTICE " + 
				"SET" + 
				"    TITLE=?," + 
				"    CONTENT=?," + 
				"    FILES=?" + 
				"WHERE ID=?";
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, title);
		st.setString(2, content);
		st.setString(3, files);
		st.setInt(4, id);
		
		int result = st.executeUpdate();
				
		st.close();
		con.close();
		
		return result;
	}
	
	public int delete(int id) throws ClassNotFoundException, SQLException {
	
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "DELETE NOTICE WHERE ID=?";
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);		
		st.setInt(1, id);
		
		int result = st.executeUpdate();
				
		st.close();
		con.close();
		
		return result;
	}

	
}

```

* spring의 data처리해주는 lib 추가(spring-jdbc)
```
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.12.RELEASE</version>
</dependency>
```
* dispatcher-servlet.xml 에 JDBCNoticeService에 dataSource property추가 및  dataSource객체 생성 후  커넥션 정보 property에 추가
```
<bean id="noticeService" class="com.newlecture.web.service.jdbc.JDBCNoticeService">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
		<property name="url" value="jdbc:oracle:thin:@localhost:1521/xepdb1"/>
		<property name="username" value="NEWLEC"/>
		<property name="password" value="24rkwk"/>
	</bean>
```
*

# Spring MVC (스프링 웹 MVC) 강의 24 - 스프링 설정파일 분리하기
* * -servlet이라는 정해진 이름, 정해진 위치(WEB-INF안)에 놔야만 했음->원하는 위치, 이름으로 저장할 수 있다?
* 설정 파일 여러개로 나눌것
* 설정 파일 나누는 이유 - 협업 시에 편하려고(각자 파트)
* service-context.xml 생성 후 service에 관련된 설정 복사
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

   <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
		<property name="url" value="jdbc:oracle:thin:@localhost:1521/xepdb1"/>
		<property name="username" value="NEWLEC"/>
		<property name="password" value="24rkwk"/>
	</bean>
	
	<bean id="noticeService" class="com.newlecture.web.service.jdbc.JDBCNoticeService">
		<property name="dataSource" ref="dataSource"/>
	</bean>
   
   
</beans>
```
* servlet-context.xml dispatcher,껍데기에 관한 내용 복사
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


   
</beans>
```
* web.cml에 dispatcher관련 설정 변경, spring이 제공하는 listner 톰캣이 시작하거나 끝나거나 이벤트를 처리할 수있는녀석? contextloader를 dispatcherservlet이 이용할 수 있다...
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">
  
  
  <listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		/WEB-INF/spring/service-context.xml
		/WEB-INF/spring/security-context.xml
	</param-value>
  </context-param>
  
  
  
  <servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>/WEB-INF/spring/servlet-context.xml</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>dispatcher</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>


  



  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to Tomcat
  </description>

</web-app>

```
* org.springframework.web.servlet.DispatcherServlet이 메모리에 올라가는 순간? url 첫번째 요청이 올 때! url요청 오기 전에 설정이 올라가있게 하는법 : <load-on-startup>1</load-on-startup>
```
  <servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>/WEB-INF/spring/servlet-context.xml</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>dispatcher</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
```
* 서블릿을 비동기로 올라가게 하는 방법 :<async-supported>true</async-supported>
```
<servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>/WEB-INF/spring/servlet-context.xml</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
  	<async-supported>true</async-supported>
  </servlet>
```

# Spring MVC (스프링 웹 MVC) 강의 25 - 객체 DI를 Annotaion으로 변경하기
* spring은 2.0부터 annotation가능
![image](https://user-images.githubusercontent.com/40667871/222437796-b245d664-dc9b-4b63-9e76-5f0e5ff3c5c7.png)

* Service객체만 annotation으로, sevlet-context.xml의 이부분 annotation으로 변경
```
<bean id="/notice/list" class="com.newlecture.web.controller.notice.ListConroller">
    	<property name="noticeService" ref="noticeService"/> <!--name 은 ListController의 setter  -->
    </bean> 
```
* listcontroller , JDBCNoticeService di를 xml에서 annotation으로 변경하는 과정
* ListController noticeService setter위에 @Autowired 추가->import까지
* servlet-context.xml 에 xmlns:context 추가
```
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd"
```
* servlet-context.xml 에 	<context:annotation-config/>추가
* autowired는 setter, constructor, 멤버변수 3가지 중 하나 위에 가능.
* JDBC가 의존하는 dataSource도 autowired로 수정
* 모든 xml파일에 context설정 해줘야한다.
* 다음 강의에서 di 뿐 아니라 객체생성도 annotation으로 대체

# Spring MVC (스프링 웹 MVC) 강의 26 - Annotation으로 서비스 객체 생성하기
* JDBCNoticeService객체 xml->annotation  : 생성할 class 위 에 @Component  (import org.springframework.stereotype.Component;)
* Component를 발견하려면 scan범위 설정해야함 package로 ->이거하면 context:annotation-config빼도된다.
```
  <context:component-scan base-package="com.newlecture.web.service"/>	
```
* Component를 세분화해서 표현하는게 Conroller, Service, Repository annotation - 역할별로 의미 보여줌

# Spring MVC (스프링 웹 MVC) 강의 27 - Annotation으로 URL 매핑하기
* annotation으로 Conroller들객체화
* servlet-context.xml에 context:conponent scan으로 package포함
* Controller에 Controller annotation 붙일 때 변경사항 ***********
* 이전 controller
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
* 변경 후 contorller
```
* servlet-context.xml 에  <mvc:annotation-driven/> 반드시 추가 ***** -> component(controller) annotation은 단순히 객체를 메모리에 올리는 일, 그 후 url맵핑은 <mvc:annotation-driven/>이 진행 -@RequestMapping("/index")와 연결되는 부분
```
* RequestMapping() annotation은 jsp파일 주소가지 맵핑해줌 view resolver이용해서
