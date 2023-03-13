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
* 공통되는 맵핑은 클래스 위에 @RequestMapping으로 올리기
* @RequestMapping은 옛날 방식이고 @GetMapping/@PostMapping으로 하는 추세
* jsp 는 WEB-INF 안에 무조건 숨겨야함 왜? controller를 통해 model을 받아서 jsp가 실행돼야 하기  때문. jsp뿐 아니라 사용자가 직접 요청해서는 안되는 다양한 설정파일들을 모두 WEB-INF안에 숨긴다.
* WEB-INF에 넣어서 요청되는 url과 달라졌기 때문에 return 문자열에 전체 경로 넣어줘야한다.
* NoticeController 변경본
```
package com.newlecture.web.controller.customer;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/customer/notice/")
public class NoticeController {
	@RequestMapping("list")
	public String list(Model model) {
		
		
		model.addAttribute("test", "Hello~");
		
		return "/WEB-INF/view/customer/notice/list.jsp";
	}
	
	@RequestMapping("detail")
	public String detail() {
		return "/WEB-INF/view/customer/notice/detail .jsp";
	}
}
```

# Sping Boot 2.x Quick Start 강의 09 - Resource View Resolver 설정하기
![image](https://user-images.githubusercontent.com/40667871/224724664-70859b93-b32c-44e2-9301-8c6b90309354.png)
![image](https://user-images.githubusercontent.com/40667871/224724778-2a104b25-044a-416e-b772-8380b10c0f2f.png)
![image](https://user-images.githubusercontent.com/40667871/224724826-54633e80-20bc-4633-97a2-3462ba5c54d1.png)


* Dispatcher(발차원) : controller 앞의 controller front controller( spring mvc에서 제공)
* view resolver : dispatcher가 특정 view를 찾아주도록 도와주는 lib
* resource view resolver : 디렉토리 내에서 특정 파일명을 갖고 찾아주는게 resource view resolver
* tiles view resolver : tiles의 구성설정에서 있는 설정에서 찾아서 view파일 반환
* application.properties 파일에서 resource view resolver설정하기
```
spring.mvc.view.prefix=/WEB-INF/view
spring.mvc.view.suffix=.jsp
```

# Sping Boot 2.x Quick Start 강의 10 - DevTools 설정하기
* 출력 문자열을 수정하면 프로그램을 다시 시작해야 한다.
* devtools lib : 코드가 수정되면 자동으로 다시 시작해주는 도구 - pom.xml에 dependency 설정 필요(spring-boot의 lib임)
```
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-devtools</artifactId>
		</dependency>
```
* 버전은 spring-boot에서 지정해줘서 빼야하는듯
