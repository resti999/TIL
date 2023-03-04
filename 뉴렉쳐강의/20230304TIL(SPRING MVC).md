# Spring MVC (스프링 웹 MVC) 강의 28 - HomController 만들기
* 3가지 눈여겨볼점
   1. IndexConroller가 @RequestMapping url에 맵핑되지 않는다는 점?->url이 클래스와 맵핑되는게 아니고 함수에 맵핑된다!!!!!! -> 한 클래스에 메서드를 여러개 놓고 각자 url맵핑을 할 수있다는 의미-> 실질적 컨트롤러는 메서드가 되는 것 클래스는 컨트롤러를 담는 큰 그릇->class이름을 폴더단위로 지어야 한다.  IndexController->HomeController/RootController
   2. data와 tile를 통해 view를 심는법

* 구 IndexConroller -> HomeController로 변경 후 메서드 이름도 변경
```
package com.newlecture.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {
	
	@RequestMapping("/index")
	public String index(){
		return "root.index";
	}
	
	
	
	
//	@Override
//	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
//		
//		
//		ModelAndView mv = new ModelAndView("root.index");
//		mv.addObject("data", "Hello Spring MVC~");
////		mv.setViewName("/WEB-INF/view/index.jsp");
//		return mv;
//	}


	
}

```

# Spring MVC (스프링 웹 MVC) 강의 29 -컨트롤러 정리하기
* RuquestMapping Annotation으로 class단위가 아닌 method단위로 url 맵핑
* 구체적으로 method와 class 나누는 기준 -> 물리 achitecture (view에 파일을 만들 때 구조 고민하는 내용)
* view단의 폴더를 기준으로 controller생성하는게 바람직
* 웬만하면 파일 구조와 url구조를 맞춰주는게 좋다.
* ListController, DetailController 삭제
* servlet-context.xml controller bean들 삭제 or 주석처리
* header.jsp 에 url 주소 customer/notice/list로 변경
* ListController 와 DetailController를 합친 NoticeController
```
package com.newlecture.web.controller.customer;

import java.sql.SQLException;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

@Controller  //객체화 해주는 annotation
@RequestMapping("/customer/notice/") //전체 url
public class NoticeController {
	
	@Autowired
	private NoticeService noticeService;
	
	@RequestMapping("list") //class의 url 뒤에 붙여진다.
	public String list() throws ClassNotFoundException, SQLException{
		
		List<Notice> list = noticeService.getList(1, "TITLE", "");

		
		return "notice.list";
	}
	
	
	@RequestMapping("detail")
	public String detail(){
		return "notice.detail";
	}
	
}

```

# Spring MVC (스프링 웹 MVC) 강의 30 -컨트롤러를 위한 Annotation 개념정리
*
