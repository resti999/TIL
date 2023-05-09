# 스프링 프레임워크 강의 7강 - 스프링 DI 지시서 작성하기(Spring Bean Configuration)
* Spring 설정파일로 DI지시하는 법
* bean으로 생성할 때 자료형은...? 생성된 객체가 구현한 인터페이스를 자동으로 ?
* property의 name 은 setExam 이 생략돼서 exam이 된 것->setter함수가 구현돼 있어야한다!. setter에 전달되는 데이터 타입이 value형이면 value속성에, reference형이면 ref속성에 변수명 입력
*  DI 지시서 예시
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<!--Exam exam = new NewlecExam();  -->
	<bean id="exam" class="spring.di.entity.NewlecExam"/>
	<!-- ExamConsole console = new InlineExamConsole(); -->
	<bean id="console" class="spring.di.ui.GridExamConsole">
	<!-- 	console.setExam(exam); injection 부분 -->
		<property name="exam" ref="exam"></property>
	</bean>
	
</beans>

```

# 스프링 프레임워크 강의 8강 - 스프링 IoC 컨테이너 사용하기(ApplicationContext 이용하기)
* 만들었던 지시서를 읽어서 지시대로 객체를 만들고 활용하는 방법
* ApplicationContext(IoC contationer의 구체적인 이름) : Spring에서 DI 지시서를 읽어서 객체를 생성해주고 조립해주는 개체(인터페이스명)
* ClassPathXmlApplicationContext("config.xml") : 실질적으로 위의 인터페이스를 구현하고 있는 개체
   * 이것 말고도 다양한 종류가 있다.(지시서 위치를 어떻게 알려주느냐 차이)
      * ClassPathXmlApplicationContext : 어플리케이션의 루트(eclipse 프로젝트의 src가 루트)로 부터 경로지정, 어플리케이션 실행 위치
      * FileSystemXmlApplicationContext : 파일시스템의 경로에 있다
      * XmlWebApplicationContext : 웹의 url을 통해 지시서 전달
      * AnnotationConfigApplicationContext  : 파일로 넘기지 않고 스캔하는 방법 사용 ->나중에 설명
* Spring library는 메이븐을 통해 따로 추가 dependency 는 version이랑 build태그 사이에
* 지시서대로 사용한 코드
```
package spring.di;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import spring.di.ui.ExamConsole;

public class Program {

	public static void main(String[] args) {
		/* 스프링에게 지시하는 방법으로 코드를 변경
		Exam exam = new NewlecExam();
		ExamConsole console = new InlineExamConsole();
		//Di인데 DI하는 것을 코드 수정이 아닌 외부 수정파일로 객체 생성부분만 전담해준다면 코드 수정없이 외부 수정파일 만 변경하고도 원하는 객체를 조립?의존성변경? 을 마음대로 할 수 잇따.
		console.setExam(exam);
		*/
		
		ApplicationContext context = new ClassPathXmlApplicationContext("spring/di/setting.xml");
		
//		ExamConsole console =(ExamConsole) context.getBean("console");
		ExamConsole console =context.getBean(ExamConsole.class);//이게 더 선호되는 방법
		
		console.print();
		
	}

}

```
* 지시서엔 <bean id="console" class="spring.di.ui.GridExamConsole"> GridExamConsole을 넣었는데 왜 사용할 땐 ExamConsole console =context.getBean(ExamConsole.class);로 인터페이스 클래스를 호출하는지?
# 스프링 프레임워크 강의 9강 - 값 형식 DI
* 지시서로 객체에 값 대입하기
```
<bean id="exam" class="spring.di.entity.NewlecExam">
		<!-- exam.setKor(20);
		exam.setEng(50);
		exam.setMath(80); -->
		<property name="kor" value="20"/>
		<property name="eng" value="50"/>
		<property name="math" value="80"/>
</bean>
```
# 스프링 프레임워크 강의 10강 - 생성자 DI(?)
* Dependency 객체 생성과 초기화
![image](https://user-images.githubusercontent.com/40667871/218768802-8306ef1a-54a8-461b-b7ab-b908f08afcc4.png)
* constructor-arg 태그의 속성에 index로 순서대로 값을 받을 수도 있고 name으로 멤버변수명으로 받을 수 있다.
*생성자 호출시 모호한 매개변수를 지닐 때
![image](https://user-images.githubusercontent.com/40667871/218770355-4828b396-6dba-4d42-99a9-c7dd03c92124.png)
* contructor-arg 태그에 type 속성사용해 명시
* 설정파일 지시자를 통해 객체 생성 초기화
![image](https://user-images.githubusercontent.com/40667871/218770684-9d8d54e7-c021-4cb0-9e5e-2a8a2ba6f588.png)
* spring config editor 안열리는 문제 발생 eclipse 2019-09
* namespace란? 프로그램과관련된 랭귀지들은 모듈이란것이 있고 모듈이 충돌나는 이름을 가질 수 있다. 이름을 식별하기 위해 붙여지는 성이 namespace인 셈,같은 문서에 두가지의 빈이 있을 때 접두사로 특정한 처리기에 의해 실행되도록 하며 태그의 이름을 식별함 태그 앞에 네임스페이스를 쓰면 그 네임스페이스에 의해 처리될 녀석이라는 뜻?
