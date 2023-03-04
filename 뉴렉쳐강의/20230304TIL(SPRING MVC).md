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
* Xml 스타일 구현상황
![image](https://user-images.githubusercontent.com/40667871/222891883-acfd3bae-79b6-444c-b35a-4c24af80457b.png)
![image](https://user-images.githubusercontent.com/40667871/222892217-4485d50a-f648-4515-93a5-c75309189388.png)
* Front Controller 만 servlet으로 만들어짐. url 을 각 pojo controller에 뿌려주는 역할
![image](https://user-images.githubusercontent.com/40667871/222892352-ff78372d-d96d-4226-b1ce-62942c2b3ad9.png)
* class는 폴더 같은 의미가 됨.
* 꼭 servlet-context에 @Controller의 객체 생성을 위한 <context:component-scan base-package="..."/>설정과 @RequestMapping의url mapping을 위한 <mvc:annotation-driven>설정이 있어야함.
* RequestMapping 의 메서드가 반환값이 없으면  mapping된 url 주소를 기반으로 viewResolver의 정보를 따라서 jsp파일을 찾는다
* resolver가 정의가 안돼있다면 index에 대한 view정보가 전혀 없어서 500에러가 난다.
![image](https://user-images.githubusercontent.com/40667871/222893504-59280fa4-c50f-448b-bf85-f6a5c85a8d17.png)
* url에 해당 class가 없을경우 404반환
![image](https://user-images.githubusercontent.com/40667871/222893546-54dc17ed-06c0-4380-92f1-fcd51918e40f.png)
* 404에러 : 페이지를 찾을 수 없음.  500에러 : url 은 있으나 server내부 에러
* string으로 반환된 값을 tiles가 받아서 jsp파일 맵핑해주는 방식

# Spring MVC (스프링 웹 MVC) 강의 31 - 문서 출력방법 4가지 @ResponseBody
![image](https://user-images.githubusercontent.com/40667871/222894564-401d1f1d-c0e7-493a-9fad-3a5ac4deed95.png)
* Front Controller에서 HttpServletResponse 전달
![image](https://user-images.githubusercontent.com/40667871/222894673-2777e241-3535-4a37-a1e9-ef9e644ffc82.png)
* 서블릿 객체 얻어서 출력하는 예제
```
package com.newlecture.web.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/")
public class HomeController {
	
	@RequestMapping("index")
	public void index(HttpServletResponse response){
		
		PrintWriter out;
		try {
			out = response.getWriter();
			out.println("Hello");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		//return "root.index";
	}
	
```
* 출력 결과
![image](https://user-images.githubusercontent.com/40667871/222894791-621abbee-df6b-46a0-8339-90e1eeb57a67.png)
* 위의 방법을 더 쉽게 해주는 @ResponseBody
* 코드 예시, 출력은 동일
```
package com.newlecture.web.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/")
public class HomeController {
	
	@RequestMapping("index")
	@ResponseBody
	public String index(HttpServletResponse response){
		
		return "Hello Index";
	}
	
	
```

# Spring MVC (스프링 웹 MVC) 강의 32 - @RestController와 한글출력 설정
*  
