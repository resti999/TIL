# Sping Boot 2.x Quick Start 강의 21 - Mapper 객체 만들기
* Maven에 Mybatis lib 추가 -> pom.xml말고도 boot starters 로 추가할 수 있다. (처음web lib추가한 것 처럼)
   * 프로젝트 왼쪽클릭
   * spring -> add starters 
      * 저번에 tilesConfiuration class import할 때 tiles 관련 lib 이 spring 버전 때문에 spring lib에 포함되지 않았던 문제를 spring boot 버전을 낮춰서 해결했는데 spring starters 를 이용하기 위해서는  2.7.9버전 이상이 필요해서 2.7.9버전으로 변경
      * sql-> Mybatis Framework, Mysql driver 체크하고 next
      * 오른쪽 화살표 클릭해서 dependency 추가
      * DAO 클래스에서 annotation 맵핑하기
* 만들어진 DAO 객체를 사용하는 방법 다음 수업때 안내
* DAO class 
```
package com.newlecture.web.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import com.newlecture.web.entity.Notice;


@Mapper //이 어노테이션을 읽어서 IoC컨테이너에 객체 담아줌
public interface NoticeDao {
	
	
	@Select("SELECT * FROM NOTICE")
	List<Notice> getList();
	
	
	Notice get(int id);

}

```
* datbase access 위한 datasource 설정, application properties
```
spring.mvc.view.prefix=/WEB-INF/view
spring.mvc.view.suffix=.jsp



# mysql settings
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/newlecture
spring.datasource.username=newlecture
spring.datasource.password=24rkwk

```

# Sping Boot 2.x Quick Start 강의 22 - 스프링에서 객체 사용방법(dependency와 injection 그리고 IoC 컨테이너)
* Dependency 생성과 사용하는 방식 3가지
![image](https://user-images.githubusercontent.com/40667871/226154888-eef59121-303d-4c7b-89d0-fc1c1f7c63de.png)
   * 첫번째 것 Composition Has a : 객체 생성 때 생성자로 객체 생성, 일체형
   * 두번째 것 Assosiation Has a : 다른 메서드 호출 때 객체 생성, 주입형 --> spring에서 가장 중요하며 사용하는 방법
* 3번째 방식만 인터페이스를 활용할 필요가 있는 객체 생성 방식
![image](https://user-images.githubusercontent.com/40667871/226155005-0eb091f8-7d8d-4f08-bfd7-380f487765cf.png)
* 객체를 조립하는 코드의 위치와 수정
![image](https://user-images.githubusercontent.com/40667871/226155034-8def445c-d840-4103-9920-e1208bc0ee85.png)
* 기업형/spring 프로그램은 App부분의 객체 조립 코드를 소스코드 수정이 필요 없도록 설정으로 빼 놓음 -> 도와주는 lib가 spring
![image](https://user-images.githubusercontent.com/40667871/226155169-d9d9adb2-14e9-4620-98c9-d61b9b9a8725.png)
* IoC컨테이너 : 객체를 생성해주고 DI를 해주는 컨테이너, 객체를 최 하위의 객체부터 생성해서 조립해서 일체형일 때는 app->?frame순으로 객체가 생성되니 Inversion of Contationer라고 명령한 것, 객체 생성이 역행해서
* 객체 생성과 Di를 요즘은 Annotation으로 함 @Component(@Controller,@Service,@Repository,@Configuration)-객체생성, @Autowired - DI
![image](https://user-images.githubusercontent.com/40667871/226155300-1ca16451-1ab2-4b76-9081-9fede2f699b9.png)

# Sping Boot 2.x Quick Start 강의 23 - Mapper 객체를 이용해서 공지목록 출력하기
* Mapper 도 객체 생성하는 역할
* customer NoticeController  NoticeService interface 멤버변수로 추가, 사용할 메서드에서 사용
* NoticeService 구현체 클래스에 Service annotation으로 객체 생성
* Notice entity 멤버변수 생성해주기
* list.jsp 데이터 받아서 jstl로 반복적으로 notice list뿌려주기




