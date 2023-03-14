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
* 과거 페이지 집중화 방식
![image](https://user-images.githubusercontent.com/40667871/225029251-a96b961e-4710-46ac-b40e-894b9d9f1d0e.png)
![image](https://user-images.githubusercontent.com/40667871/225029359-2de73ef0-18e4-476f-aeb8-e0ae3d877980.png)
* main page말고 주위의 틀조차 include문을 복사붙여넣기 하기 번거로움 -> 페이지가 많아지면 그만큼 일을 해야함
* tiles lib로 처리할 경우
![image](https://user-images.githubusercontent.com/40667871/225029722-7581d155-5ae6-4c3f-b027-e5db32c5cbda.png)
* tiles 는 apache에서 제공하는 lib(은퇴해서 홈페이지에 없음),  프론트단에서 timeleaf라는 페이지 모듈화 툴이 있음
* tiles 설정 갖고오는법
   * apache.org ->Attic(은퇴 or 완벽(Tiles) ->tiles-> doc 3.0 ->tutorial ->creating tiles page->create a definition xml 빼고 복사->WEB-INF/tiles.xml 에 저장
* MVC Model2 Tiles
![image](https://user-images.githubusercontent.com/40667871/225032653-a8a02fbb-1a8e-4298-9fd6-96b8eceb9730.png)
* tiles lib설정과 객체화 과정 필요(다음 수업)

# Sping Boot 2.x Quick Start 강의 15 - Tiles View Resolver 생성하기
* viewResolver가 여러개일 때 우선순위 정해줘야함
![image](https://user-images.githubusercontent.com/40667871/225035040-62bf560e-159d-4497-a300-583998352dd7.png)
* IoC에 담게될 객체들을 반환해주는 함수들 , 클래스이름은 아무거나 상관없음- get~, create이름이 아니고 객체의 이름과 동일.
* tiles lib, jstl lib 추가 pom.xml dependency
```
		<!-- https://mvnrepository.com/artifact/org.apache.tiles/tiles-jsp -->
		<dependency>
		    <groupId>org.apache.tiles</groupId>
		    <artifactId>tiles-jsp</artifactId>
		    <version>3.0.8</version>
		</dependency>
		
		<!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
		<dependency>
		    <groupId>javax.servlet</groupId>
		    <artifactId>jstl</artifactId>
		    <version>1.2</version>
		</dependency>
		
```
* TileConfiguration 클래스
```
package com.newlecture.web.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.view.tiles3.TilesConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesView;
import org.springframework.web.servlet.view.tiles3.TilesViewResolver;


@Configuration
public class TilesConfig {
     @Bean
     public TilesConfigurer tilesConfigurer(){
      TilesConfigurer tilesConfigurer = new TilesConfigurer();
      tilesConfigurer.setDefinitions(new String[] { "/WEB-INF/tiles.xml"} );
      tilesConfigurer.setCheckRefresh(true);
      return tilesConfigurer;
    }

    @Bean
    public TilesViewResolver tilesViewResolver(){
       TilesViewResolver  viewResolver = new TilesViewResolver();
       viewResolver.setViewClass(TilesView.class);
       viewResolver.setOrder(1);
       return viewResolver;
   }    
}

```
* 해당 클래스 import 안되는 문제 해결
```
import org.springframework.web.servlet.view.tiles3 했을때 view밑에 tiles3 패키지 자체가 없을 수 있습니다.  그냥 스프링부트 생성하면 3.x 로 3점대꺼 깔아주는거같은데 이게 tiles가 업데이트를 워낙 안하다보니 버전 호환이 안되는거같은데, 이제와서 tiles가 업데이트 하지는 않을거같고 pom.xml에서 스프링부트 버전을 2.6.1같은걸로 낮추고 해보실래요?
```
* tile.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
       "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"
       "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
  <definition name="customer.notice.*" template="/WEB-INF/view/customer/inc/layout.jsp">
    <put-attribute name="header" value="/WEB-INF/view/inc/header.jsp" />
    <put-attribute name="visual" value="/WEB-INF/view/customer/inc/visual.jsp" />
    <put-attribute name="aside" value="/WEB-INF/view/customer/inc/aside.jsp" />
    <put-attribute name="main" value="/WEB-INF/view/customer/notice/{1}.jsp" />
    <put-attribute name="footer" value="/WEB-INF/view/inc/footer.jsp" />
  </definition>
</tiles-definitions>
```
* NoticeController 변경사항 반환부분 tile형식으로
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
		
		
		model.addAttribute("test", "Hello~DevTools-asdfasdf");
		
//		return "/customer/notice/list"; ResourceViewResolver
		return "customer.notice.list";  // TilesViewResolver
	}
	
	@RequestMapping("detail")
	public String detail() {
		return "/customer/notice/detail";
	}
}

```
*
