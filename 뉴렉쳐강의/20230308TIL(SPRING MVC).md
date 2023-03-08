# Spring MVC (스프링 웹 MVC) 강의 45 - POST 입력 #7(다중파일 업로드)
* 첨부파일 input이 2개 일 경우? name이 같으면 배열로 전달됨.
* Multipart배열로 받고 files로 받는다. 그 후 전 예제에서 for문만 추가하면 된다.
* NoticeController 변경사항
```
package com.newlecture.web.controller.customer.admin.board;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController { // <bean name="noticeController" classes=""
	
	@RequestMapping("list")
	public String list() {
		return "";
	}
	
	@RequestMapping("reg")
	@ResponseBody
	public String reg(String title, String content,MultipartFile[] files, String category, String[] foods, String food, HttpServletRequest request) throws IllegalStateException, IOException {
		
		for(MultipartFile file : files) {
			long size = file.getSize();
			String fileName = file.getOriginalFilename();
			System.out.printf("fileName:%s, fileSize:%d\n", fileName, size);
			
			//파일 저장을 위해 경로지정
			ServletContext ctx=request.getServletContext();
			String webPath = "/static/upload";
			String realPath = ctx.getRealPath(webPath);
			System.out.printf("realPath: %s\n", realPath);
			//파일 업로드할 폴더 있는지 확인
			File savePath = new File(realPath);
			if(!savePath.exists()) // 경로 실제 존재 확인 -> 경로가 없으면
				savePath.mkdirs(); // mkdir와 다르게 중간에 빠진 경로 있으면 다 만들어줌
			//os별 path 구분자로 경로 뒤에 파일 이름 넣기
			realPath+=File.separator+fileName;
			File saveFile = new File(realPath);
			//multipart 객체를 통해 파일 저장
			file.transferTo(saveFile);
		}
		
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return String.format("title:%s<br> content:%s<br>category:%s<br>food:%s", title, content,category,food);
	}
	
	@RequestMapping("edit")
	public String edit() {
		return "";
	}
	
	@RequestMapping("del")
	public String del() {
		return "";
	}
}

```

# Spring MVC (스프링 웹 MVC) 강의 46 - 공지사항 관리자 페이지 준비하기
* 뉴렉처사이트에서 스프링 mvc 관리자 view페이지 다운
* 이제 문자를 반환하는 것이 아니기 때문에 NoticeController의 reg함수 @ResponseBody 지우고 return "admin.board.notice.list"로 변경
* tile에서 wildcard는 여러개 사용 가능
* tiles.xml설정하기
```
<definition name="admin.board.*.*" template="/WEB-INF/view/admin/inc/layout.jsp">
    <put-attribute name="title" value="공지사항" />
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/admin/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/admin/inc/aside.jsp" />
    <put-attribute name="body" value="/WEB-INF/view/admin/board/{1}/{2}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
```
* NoticeController   변경사항
```
package com.newlecture.web.controller.customer.admin.board;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController { // <bean name="noticeController" classes=""
	
	@RequestMapping("list")
	public String list() {
		return "admin.board.notice.list";
	}
	
	@RequestMapping("reg")
	public String reg(String title, String content,MultipartFile[] files, String category, String[] foods, String food, HttpServletRequest request) throws IllegalStateException, IOException {
		
		for(MultipartFile file : files) {
			long size = file.getSize();
			String fileName = file.getOriginalFilename();
			System.out.printf("fileName:%s, fileSize:%d\n", fileName, size);
			
			//파일 저장을 위해 경로지정
			ServletContext ctx=request.getServletContext();
			String webPath = "/static/upload";
			String realPath = ctx.getRealPath(webPath);
			System.out.printf("realPath: %s\n", realPath);
			//파일 업로드할 폴더 있는지 확인
			File savePath = new File(realPath);
			if(!savePath.exists()) // 경로 실제 존재 확인 -> 경로가 없으면
				savePath.mkdirs(); // mkdir와 다르게 중간에 빠진 경로 있으면 다 만들어줌
			//os별 path 구분자로 경로 뒤에 파일 이름 넣기
			realPath+=File.separator+fileName;
			File saveFile = new File(realPath);
			//multipart 객체를 통해 파일 저장
			file.transferTo(saveFile);
		}
		
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return "admin.board.notice.reg";
	}
	
	@RequestMapping("edit")
	public String edit() {
		return "admin.board.notice.edit";
	}
	
	@RequestMapping("del")
	public String del() {
		return "admin.board.notice.list";
	}
}

```
* reg는 get요청 땐 요청인자 없어도 됨. post get요청을 나눌 예정

# Spring MVC (스프링 웹 MVC) 강의 47 - POST 매핑과 Redirection
* get, post 구분하는법 doGet, doPost함수
![image](https://user-images.githubusercontent.com/40667871/223719932-4e66ebbb-1cf8-4c69-87ee-af2313538e85.png)
* Post는 Get요청과 달리 redirect필요 (get에선 forwarding)
* Spring 4ver이상에서의 get, post mapping
![image](https://user-images.githubusercontent.com/40667871/223720310-805b3505-f34e-4b28-8393-d38ea82fd77b.png)
* list.jsp의 link 들 html 다 빼기
* NoticeController 변경사항 : @GetMapping , @PostMapping
```
package com.newlecture.web.controller.customer.admin.board;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController { // <bean name="noticeController" classes=""
	
	@RequestMapping("list")
	public String list() {
		return "admin.board.notice.list";
	}
	
	@GetMapping("reg")
	public String reg(){
		return "admin.board.notice.reg"; 
	}
	
	
	@PostMapping("reg")
	public String reg(String title, String content,MultipartFile[] files, String category, String[] foods, String food, HttpServletRequest request) throws IllegalStateException, IOException {
		
		for(MultipartFile file : files) {
			long size = file.getSize();
			String fileName = file.getOriginalFilename();
			System.out.printf("fileName:%s, fileSize:%d\n", fileName, size);
			
			//파일 저장을 위해 경로지정
			ServletContext ctx=request.getServletContext();
			String webPath = "/static/upload";
			String realPath = ctx.getRealPath(webPath);
			System.out.printf("realPath: %s\n", realPath);
			//파일 업로드할 폴더 있는지 확인
			File savePath = new File(realPath);
			if(!savePath.exists()) // 경로 실제 존재 확인 -> 경로가 없으면
				savePath.mkdirs(); // mkdir와 다르게 중간에 빠진 경로 있으면 다 만들어줌
			//os별 path 구분자로 경로 뒤에 파일 이름 넣기
			realPath+=File.separator+fileName;
			File saveFile = new File(realPath);
			//multipart 객체를 통해 파일 저장
			file.transferTo(saveFile);
		}
		
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return "admin.board.notice.reg";
	}
	
	@RequestMapping("edit")
	public String edit() {
		return "admin.board.notice.edit";
	}
	
	@RequestMapping("del")
	public String del() {
		return "admin.board.notice.list";
	}
}

```

# Sping Boot 2.x Quick Start 강의 01 - 스프링 부트란?
* spring은  was 위에 servlet/jsp 위에 얹어지는 프레임워크
* ![image](https://user-images.githubusercontent.com/40667871/223723333-32dcbc5f-ac22-42d7-99f7-4c572fc8a141.png)
* was xml 설정, servlet/jsp 설정, spring 설정 , lib설정등이 번거로움
* was 설치하지마라, servlet/jsp 필요한 lib세팅, spring관련 lib들 하나로 모아놓음 -> boot
* spring dependency를 하나로 뭉쳐놓음, 최소한의 configure
* 기존의 Spring Framework를 이용한 웹 개발
![image](https://user-images.githubusercontent.com/40667871/223724425-1c57adb4-f1a6-4b89-8b34-af80a5865bde.png)
* boot가 해결해주는것
![image](https://user-images.githubusercontent.com/40667871/223724622-a1a9b2c0-0bc5-492a-9a10-6548dd72d1c3.png

# Sping Boot 2.x Quick Start 강의 02 - 학습개요
![image](https://user-images.githubusercontent.com/40667871/223725607-71cca0b3-0871-4cb3-96e1-7ba008233b05.png)
* mvc 기반으로 service, dao 계층 만듬
* view page는 jsp 이후 timeleaf도 살펴봄
* java라는 runtime환경에서 main 언어는 java
* persistence(영속성,데이터 db에 넣는 작업) 은 Mybatis 후, JPA+Hibernate
* db는 오라클
* 인증, 권한 담당은 spring security
![image](https://user-images.githubusercontent.com/40667871/223726237-2f1708e1-106c-49a7-a630-4d4b9f5b3365.png)


