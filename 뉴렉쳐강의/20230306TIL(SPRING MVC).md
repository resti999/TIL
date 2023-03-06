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
* 

