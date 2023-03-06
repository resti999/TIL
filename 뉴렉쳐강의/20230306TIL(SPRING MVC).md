# Spring MVC (스프링 웹 MVC) 강의 35 - QueryString 입력 #1
* 입력 도구를 얻어오는 방법
![image](https://user-images.githubusercontent.com/40667871/223104753-437e7aa4-dc74-4fe5-ac8f-f4bb04758515.png)
![image](https://user-images.githubusercontent.com/40667871/223107916-e0598978-917a-457d-8a39-c692ee63822a.png)
String p(쿼리 변수 이름과 같아야함)만 해도 클라이언트 요청값을 가져올 수 있다.

# Spring MVC (스프링 웹 MVC) 강의 36 - QueryString 입력 #2 : 변수명 별칭과 기본 값처리
* query string으로 온 이름 말고 다른 이름으로 받고 싶을 경우(query string은 길면 안돼서 보통 축약해서 오기 때문) -? 매개변수 앞에 @RequestParam() annotation사용
![image](https://user-images.githubusercontent.com/40667871/223110208-abd3810f-4a6e-4662-ae77-101369611ff5.png)
* 위의 방법은 인자 전달 안 할 경우 에러발생 -> 해결법 : 기본값 지정 @RequestParam(defaultValue="1")
* @RequestParma("p") 는 @RequestParam(name="p") 와 같다.(name속성이 생략된 것)
* Query String으로 전달되는 parameter는 무조건 string으로 오지만 spring에선 바로 int로 받을 수 있음
   * 예시 : Public String list(@RequestPara(name="p") int page) 

# Spring MVC (스프링 웹 MVC) 강의 37 - @RequestParam Optional 속성
* spring doc : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html
* RequestParma 속성들
![image](https://user-images.githubusercontent.com/40667871/223113010-99255b7c-5c55-4ca6-8ed8-477d32cfa63b.png)
* default값 없어도 required속성을 false로 변경하면 오류나지 않음
* required false로 한 후 query string parameter값을 전달하지 않는다면 null값을 받을 수 있는 자료형으로 매개변수를 선언해야한다.int->Integer
* value속성은 name속성의 별칭, 두개 동시에 속성설정하면안됨

# Spring MVC (스프링 웹 MVC) 강의 38 - POST 입력을 위한 Admin 컨트롤러 추가하기
* bean의 이름은 클래스명을 기반으로 붙여지는데 같은 이름의 bean이름이 있으면 오류남.-> 클래스이름이 같으면 bean이름 따로 설정해줘야함

# Spring MVC (스프링 웹 MVC) 강의 39 - POST 입력방법
* root안의 static폴더에 접근 못하는 이유 /static을 /로 맵핑해놔서 : <mvc:resources location="/static/" mapping="/**"></mvc:resources>설정
* /admin/board/notice/reg.html 하면 reg문자만 나오는 이유 : RequestMapping("reg") 는 정확히 url reg를 맵핑해주는게 아니고 패턴이다.-> reg들어간 url 모두 맵핑
* 실습과정
   * reg1. html 에form 의 action속성을 reg로
   ```
   <form action="reg" method="post">
                    <div class="margin-top first">
                        <h3 class="hidden">공지사항 입력</h3>
                        <table class="table">
                            <tbody>
                                <tr>
                                    <th>제목</th>
                                    <td class="text-align-left text-indent text-strong text-orange" colspan="3">
                                        <input type="text" name="title" />
                                    </td>
                                </tr>
                                <tr>
                                    <th>첨부파일</th>
                                    <td colspan="3" class="text-align-left text-indent"><input type="file"
                                            name="file" /> </td>
                                </tr>
                                <tr class="content">
                                    <td colspan="4"><textarea class="content" name="content"></textarea></td>
                                </tr>
                                <tr>
                                    <td colspan="4" class="text-align-right"><input class="vertical-align" type="checkbox" id="open" name="open" value="true"><label for="open" class="margin-left">바로공개</label> </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="margin-top text-align-center">
                        <input class="btn-text btn-default" type="submit" value="등록" />
                        <a class="btn-text btn-cancel" href="list.html">취소</a>
                    </div>
                </form>
   ```
   *  admin NoticeController 의 reg method("reg" url맵핑된) reg1.html에서 온 post 데이터 매개변수로 받아서 출력
   ```
   package com.newlecture.web.controller.customer.admin.board;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController { // <bean name="noticeController" classes=""
	
	@RequestMapping("list")
	public String list() {
		return "";
	}
	
	@RequestMapping("reg")
	@ResponseBody
	public String reg(String title, String content) {
		return String.format("title:%s<br> content:%s", title, content);
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
