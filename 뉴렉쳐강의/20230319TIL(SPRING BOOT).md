# Sping Boot 2.x Quick Start 강의 21 - Mapper 객체 만들기
* Maven에 Mybatis lib 추가 -> pom.xml말고도 boot starters 로 추가할 수 있다. (처음web lib추가한 것 처럼)
   * 프로젝트 왼쪽클릭
   * spring -> add starters 
      * 저번에 tilesConfiuration class import할 때 tiles 관련 lib 이 spring 버전 때문에 spring lib에 포함되지 않았던 문제를 spring boot 버전을 낮춰서 해결했는데 spring starters 를 이용하기 위해서는  2.7.9버전 이상이 필요해서 2.7.9버전으로 변경
      * sql-> Mybatis Framework, Mysql driver 체크하고 next
      * 오른쪽 화살표 클릭해서 dependency 추가
      * DAO 클래스에서 annotation 맵핑하기
* 만들어진 DAO 객체를 사용하는 방법 다음 수업때 안내
* DAO class 
```
package com.newlecture.web.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import com.newlecture.web.entity.Notice;


@Mapper //이 어노테이션을 읽어서 IoC컨테이너에 객체 담아줌
public interface NoticeDao {
	
	
	@Select("SELECT * FROM NOTICE")
	List<Notice> getList();
	
	
	Notice get(int id);

}

```
* datbase access 위한 datasource 설정, application properties
```
spring.mvc.view.prefix=/WEB-INF/view
spring.mvc.view.suffix=.jsp



# mysql settings
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/newlecture
spring.datasource.username=newlecture
spring.datasource.password=24rkwk

```

# Sping Boot 2.x Quick Start 강의 22 - 스프링에서 객체 사용방법(dependency와 injection 그리고 IoC 컨테이너)
* Dependency 생성과 사용하는 방식 3가지
![image](https://user-images.githubusercontent.com/40667871/226154888-eef59121-303d-4c7b-89d0-fc1c1f7c63de.png)
   * 첫번째 것 Composition Has a : 객체 생성 때 생성자로 객체 생성, 일체형
   * 두번째 것 Assosiation Has a : 다른 메서드 호출 때 객체 생성, 주입형 --> spring에서 가장 중요하며 사용하는 방법
* 3번째 방식만 인터페이스를 활용할 필요가 있는 객체 생성 방식
![image](https://user-images.githubusercontent.com/40667871/226155005-0eb091f8-7d8d-4f08-bfd7-380f487765cf.png)
* 객체를 조립하는 코드의 위치와 수정
![image](https://user-images.githubusercontent.com/40667871/226155034-8def445c-d840-4103-9920-e1208bc0ee85.png)
* 기업형/spring 프로그램은 App부분의 객체 조립 코드를 소스코드 수정이 필요 없도록 설정으로 빼 놓음 -> 도와주는 lib가 spring
![image](https://user-images.githubusercontent.com/40667871/226155169-d9d9adb2-14e9-4620-98c9-d61b9b9a8725.png)
* IoC컨테이너 : 객체를 생성해주고 DI를 해주는 컨테이너, 객체를 최 하위의 객체부터 생성해서 조립해서 일체형일 때는 app->?frame순으로 객체가 생성되니 Inversion of Contationer라고 명령한 것, 객체 생성이 역행해서
* 객체 생성과 Di를 요즘은 Annotation으로 함 @Component(@Controller,@Service,@Repository,@Configuration)-객체생성, @Autowired - DI
![image](https://user-images.githubusercontent.com/40667871/226155300-1ca16451-1ab2-4b76-9081-9fede2f699b9.png)

# Sping Boot 2.x Quick Start 강의 23 - Mapper 객체를 이용해서 공지목록 출력하기
* mysql은 datasource설정 때 servertimezone설정 해야함
* Datasource application.properties에 설정해도 에러나면서 안될 때
    * 에러 내용: Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured. Reason: Failed to determine a suitable driver class
    * Configuration class로 대체
    ```
    package com.newlecture.web.config;

import javax.sql.DataSource;

import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DBConfiguration {
	
	@Bean
	public DataSource datasource() {
		return DataSourceBuilder.create()
				.driverClassName("oracle.jdbc.driver.OracleDriver")
				.url("jdbc:oracle:thin:@localhost:1521/xepdb1")
				.username("newlec")
				.password("24rkwk")
				.build();
	}
}

    ```
* Mapper 도 객체 생성하는 역할
* customer NoticeController  NoticeService interface 멤버변수로 추가, 사용할 메서드에서 사용
```
package com.newlecture.web.controller.customer;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

@Controller
@RequestMapping("/customer/notice/")
public class NoticeController {
	
	@Autowired
	private NoticeService service;
	
	
	@RequestMapping("list")
	public String list(Model model) {
		
		List<Notice> list = service.getList();
		
		model.addAttribute("list", list);
		
//		return "/customer/notice/list"; ResourceViewResolver
		return "customer.notice.list";  // TilesViewResolver
	}
	
	@RequestMapping("detail")
	public String detail() {
		return "customer.notice.detail";
	}
}

```
* NoticeService 구현체 클래스에 Service annotation으로 객체 생성 및 DAO interface autowired
```
package com.newlecture.web.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.newlecture.web.dao.NoticeDao;
import com.newlecture.web.entity.Notice;


@Service
public class NoticeServiceImp implements NoticeService {
	
	@Autowired
	private NoticeDao noticeDao;
	
	
	@Override
	public List<Notice> getList() {
		
		List<Notice> list = noticeDao.getList();
		
		
		return list;
	}

	@Override
	public Notice get(int id) {
		
		Notice notice = noticeDao.get(id);
		
		return notice;
	}
	
}

```
* Notice entity 멤버변수 생성해주기 - db 테이블과 컬럼수, 자료형 동일하게, getter,setter field 사용 생성자 생성
```
package com.newlecture.web.entity;

import java.util.Date;

public class Notice {
	private int id;
	private String title;
	private String writerId;
	private String content;
	private Date regdate;
	private int hit;
	private String files;
	private boolean pub;
	
	public Notice() {
	}

	public Notice(int id, String title, String writerId, String content, Date regdate, int hit, String files,
			boolean pub) {
		super();
		this.id = id;
		this.title = title;
		this.writerId = writerId;
		this.content = content;
		this.regdate = regdate;
		this.hit = hit;
		this.files = files;
		this.pub = pub;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getWriterId() {
		return writerId;
	}

	public void setWriterId(String writerId) {
		this.writerId = writerId;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public Date getRegdate() {
		return regdate;
	}

	public void setRegdate(Date regdate) {
		this.regdate = regdate;
	}

	public int getHit() {
		return hit;
	}

	public void setHit(int hit) {
		this.hit = hit;
	}

	public String getFiles() {
		return files;
	}

	public void setFiles(String files) {
		this.files = files;
	}

	public boolean isPub() {
		return pub;
	}

	public void setPub(boolean pub) {
		this.pub = pub;
	}

	@Override
	public String toString() {
		return "Notice [id=" + id + ", title=" + title + ", writerId=" + writerId + ", content=" + content
				+ ", regdate=" + regdate + ", hit=" + hit + ", files=" + files + ", pub=" + pub + "]";
	}

	

	

	
}

```
* list.jsp 데이터 받아서 jstl로 반복적으로 notice list뿌려주기
```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<c:forEach var="n" items="${list}">
					<tr>
						<td>${n.id}</td>
						<td class="title indent text-align-left"><a href="detail.html">${n.title}</a></td>
						<td>${n.writerId}</td>
						<td>
							${n.regdate}	
						</td>
						<td>${n.hit}</td>
					</tr>
					</c:forEach>	
```

# 



