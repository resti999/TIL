# Spring JDBC 강의 01 - Service와 Dao 레이어의 이해
![image](https://user-images.githubusercontent.com/40667871/224024253-828d7b66-8cc5-4119-ab4f-4b4585f2cfb4.png)
![image](https://user-images.githubusercontent.com/40667871/224024584-9b820ce4-b211-43db-8ee1-ae00ed58cfdf.png)
![image](https://user-images.githubusercontent.com/40667871/224024730-bb3441d4-6f39-4ed6-8244-31b4546d2899.png)
* dao를 쓴다는건 sql를 service에서 하지 않는다는것
![image](https://user-images.githubusercontent.com/40667871/224025042-4d2190cd-5b74-46d0-ad47-dddd28c5f496.png)
* dao를 쓸 때의 장점 service를 구현하는 사람은 sql을 몰라도 됨->분업을 위해\
* Controller입장에서 NoticeService는 하나의 총판이다?
![image](https://user-images.githubusercontent.com/40667871/224026094-bf2c5f30-0cf2-471b-abe8-665e5e73a9be.png)
* 전체 골격
![image](https://user-images.githubusercontent.com/40667871/224026144-4656518c-784f-47ad-b3e9-5ca0e9a4c0e6.png)
* dao는 databse table과 1:1관계를 맺는다
* dao를 통해 table을 조작할 때 단조롭고 반복적이고 코드량만 많다-> 해결해주는 lib jdbc, mybatis, jpa/hb등이 있다.

# Spring JDBC 강의 02 - JdbcTemplate을 이용해 getList 메소드 구현하기
* 그냥 jdbc와 spring jdbc코드량 차이
* spring에서 model을 통해 view로 값 전달하는 방법.
![image](https://user-images.githubusercontent.com/40667871/224027885-2613131a-5c95-470a-bd50-80218242e486.png)
* JDBC Template 사용법
![image](https://user-images.githubusercontent.com/40667871/224029507-a01a36d3-0be6-467a-9ee2-e7149dacc27c.png)

# Sping Boot 2.x Quick Start 강의 03 - Spring Tools 4 for Eclipse 다운로드
* Spring 다른 강의에서 다운 완료

# Sping Boot 2.x Quick Start 강의 04 - Spring Boot Starter 프로젝트 만들기
* spring starter project 생성
* tomcat을 통해 servlet을 사용할 때는 main함수를 사용안했음. spring boot를 사용하면 tomcat도 spring안으로 포함돼서 main함수가 생긴다.
```
package com.newlecture.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringWebApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringWebApplication.class, args);
	}

}

```
* controller 만드는법 : 무조건 기본패키지(처음설정한-예제에서 com.newlecture.web) 이후 계층에 클래스를 만들어야한다. 
* 아무 설정없이 어노테이션만으로 가능하다.
```
package com.newlecture.web.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {
	
	@RequestMapping("/index")
	public String adsf() {
		return "Hello Spring Boot";
	}
}

```
* spring boot를 실행하는 방법 : main 함수에서 ctrl+f11  -> 기존 spring은 톰캣을 실행해야만 했음
* 완전 편리하다

# Sping Boot 2.x Quick Start 강의 05 - 수업용 HTML 파일 준비하기
* 스프링 웹 MVC 강의용 html.zip파일 다운
* spring boot는 src 밑에 main 밑에 webapp(root폴더)를 직접 생성해줘야한다?wepapp은 동적인 파일(jsp등)들의 home dir -> spring boot의 이미지, js,css,html등의 static파일들은  src/main/resources/static폴더 안에 넣는다.(고정)
* static 폴더에 정적파일들 넣었는데 로드 안된 문제 해결
   * 정적 리소스 location종류는 총 4가지 , ResourceProperties에 정의되어있다.
      * classpath:/META-INF/resources/
      * classpath:/resources/
      * classpath:/static/
      * classpath:/public/ ->  src/main/resources 안에 public폴더를 만들고 그 안에 넣었더니 root로 잡히면서 로드가 됐음
