# Sping Boot 2.x Quick Start 강의 11 - 실습 : 관리자 페이지를 위한 공지사항 페이지 추가해보기
* spring에서 이름이 같은 클래스가 다른곳에서 만들어지면 오류가 발생-> bean id같아서
* admin NoticeController 생성
```
package com.newlecture.web.controller.admin.board;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController {
	
	
	@RequestMapping("list")
	public String list() {
		return "/admin/board/notice/list.jsp";
	}
	
	
	@RequestMapping("detail")
	public String detail() {
		return "admin/board/notice/detail.jsp";
	}
	
	@RequestMapping("reg")
	public String reg() {
		return "admin/board/notice/reg.jsp";
	}
	
	@RequestMapping("del")
	public String del() {
		return "";
	}
	
}

```

# Sping Boot 2.x Quick Start 강의 12 - 설명 : 관리자 페이지를 위한 공지사항 페이지 추가해보기
* 전 강의 내용

# Sping Boot 2.x Quick Start 강의 13 - 페이지 공통분모 집중화
* 일반적 페이지 
![image](https://user-images.githubusercontent.com/40667871/225013024-140b05b0-d189-47b6-bde4-349656f4354b.png)
* 가장 중요한 페이지(알맹이)는 맨 끝에 있는 페이지
![image](https://user-images.githubusercontent.com/40667871/225023896-79c0d9c5-014f-4fd8-a074-c3b3e16094da.png)
* 코드 수정을 적게하기 위해서
* 집중화 도구 tiles
![image](https://user-images.githubusercontent.com/40667871/225024302-a60b5d4e-49fa-4f5b-b4c5-e96be79272b8.png)
* 기존에는 <jsp:include page=" 부분페이지경로"/> 로 jsp파일을 도중에 포함시켰다
* customer page들과 admin page들의 main, header, footer, layout,aside, visual로 파일 나눔
![image](https://user-images.githubusercontent.com/40667871/225028889-00f37d83-138b-488e-a9f6-13635a3a2c51.png)

# Sping Boot 2.x Quick Start 강의 14 - Tiles 지시서 작성하기
* 
