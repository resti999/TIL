# Sping Boot 2.x Quick Start 강의 06 - View를 인식시키는 Jasper 라이브러리 추가하기
* webapp(root)에 index.jsp를 요청해도 다운로드만됨 -> boot의 tomcat의 jsp처리 가능한 lib없어서 -> jasper lib추가해야한다.
* maven의 pom.xml에 dependency추가 : 버전은 spring boot에서 맞게 설정해주기 때문에 제거한다.
```
<dependency>
		    <groupId>org.apache.tomcat.embed</groupId>
		    <artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
```

# Sping Boot 2.x Quick Start 강의 07 - 사용자 공지를 위한 MVC 구현하기
* customer/notice/list가 있을 때 url과 controller를 나누는 기준 - 마지막에 오는 url이 행위에 대한것. list, detail, reg,edit 등. 행위들을 담는 것들을 담는 notice클래스. notice상위는 패키지로 구현
* Controller 요청 url과 실제 jsp 파일의 폴더명을 일치시키는게 바람직하다.
* @RestController는 컨트롤러 메서드들이 반화하는 것들을 그냥 문자열로 반환. view page로 forward할 때는 @Controller로 설정해야한다.
* view page를 요청할 때 컨트롤러(메서드)의 url경로와 일치하면 파일명만 입력해도 된다.
* 단순히 controller(메서드)에서 매개변수로 Model을 선언하면 spring에서(dispatcher에서) Model객체를 생성해서 참조형식으로 보내줌
* NoticeController MVC 예시 ->  맵핑 annotation, view위치 변경 필요
```
package com.newlecture.web.controller.customer;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class NoticeController {
	@RequestMapping("/customer/notice/list")
	public String list(Model model) {
		
		
		model.addAttribute("test", "Hello~");
		
		return "list.jsp";
	}
	
	@RequestMapping("/customer/notice/detail")
	public String detail() {
		return "detail.jsp";
	}
}

```

# Sping Boot 2.x Quick Start 강의 08 - Mapping, View 위치 바로 잡기
