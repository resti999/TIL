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
