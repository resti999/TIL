# Sping Boot 2.x Quick Start 강의 38 - Mapper 객체 단위 Test with jUnit5와 XML에 resultMap 사용하기
* mapping 한 mybatis query들이 잘 동작하는지 JUNIT lib로 단위테스트
* mybatis spring boot starter test lib추가 후 클래스 만들어서 그 위에 @MybatisTest annotation 붙이고 테스트 하고자 하는 dao Autowired annotation 사용
![image](https://user-images.githubusercontent.com/40667871/227234411-2caf0535-5b1a-44e4-be1e-ea297f3581b1.png)
* mybatis-spring-boot-test 받는법
   * 구글에 mybatis spring boot test검색 
   * 들어가서 maven 복사 후 pom.xml붙여넣기
   ```
   <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter-test</artifactId>
    <version>3.0.1</version>
    <scope>test</scope>
  </dependency>
   ```
* NoticeDao 테스트할 클래스 만들기
   *  NoticeDao Interface파일 우클릭(perpective를 java ee가 아닌 java로 설정해야한다.)
   *  New->JUnit Test Case클릭
   *  Finish
   *  com.newlecture.web.dao.NoticeDaoTest 생성됨
   *  실행법 run as junit
* 0808이어서  
