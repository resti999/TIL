# 스프링 프레임워크 강의 2강 - 느슨한 결합력과 인터페이스
* 코드 수정을 없애고 DI를 위한 설정
![image](https://user-images.githubusercontent.com/40667871/218460575-bbec241e-351d-4276-9155-7a5ec08a82ef.png)
![image](https://user-images.githubusercontent.com/40667871/218460508-41551e1b-40c2-4e6d-bc60-73010d760c8f.png)
* 코드 수정이 필요하다 -> 결합력이 높다.
* Service에서 사용하는 자료형을 인터페이스 B로. 객체생성은 className함수를 이용. DAo부분에선 인터페이스 B를 계승하는 b1, b2를 갈아낌
* 객체 생성을 외부 설정파일(XML, Annotation)으로 하는것 -> DI, 결합력 낮추기

# 스프링 프레임워크 강의 3강 - DI(Dependency Injection)
* 종속 객체를 생성 조립해주는 도구 : DI(Dependency Injection), IoC Container
* 프로그램은 객체들의 조립관계를 통해서 만들어진다.
* 의존성 - Has A 관계
   * Compostion has a- 부품, 객체안에서 객체 직접 생성
   * Association has a - 멤버변수로 객체두고 객체 생성은 외부에서 생성
* 일체형 그림표현
 ![image](https://user-images.githubusercontent.com/40667871/218466806-8d0f4691-d754-4be4-bca5-ad7bba1c722d.png)
* 조립형 그림표현
![image](https://user-images.githubusercontent.com/40667871/218466849-96e01f50-c900-4d66-9abf-f58bacd0da27.png)
![image](https://user-images.githubusercontent.com/40667871/218467205-955a1efa-418d-4b2d-a4da-55426a829455.png)
* dependency 주입 2가지
   * setter함수를 통해( Setter Injection)
   * 생성자를 통해 (Construction Injection)

# 스프링 프레임워크 강의 4강 - IoC(Inversion Of Control) 컨테이너
* 객체에 대한 관계에 관한 명세/주문서(XML/Annotation)
* 명세에 따라서 객체를 생성 후 그릇(container)에 담는다.
* IoC(Inversion of Control) Container : 객체들을 생성하고 관계에 따라 조립해줌.(의존성 맺어줌)
   * 작은부품부터 만들어짐. -> 의존성이 일체형일 경우는 A만들어지고 A가 B를 만들고 B가 C를 만듦
   * 결합형으로 만들어지는 경우는 생성 순서가 역순
   ![image](https://user-images.githubusercontent.com/40667871/218469289-4fada8ab-39f3-4118-a7c8-0a314c6d7b54.png)
   
# 스프링 프레임워크 강의 5강 - Dependency를 직접 Injection하기
* 스프링 없이 DI해보기
* Program(main 함수)
```
package spring.di;

import spring.di.entity.Exam;
import spring.di.entity.NewlecExam;
import spring.di.ui.ExamConsole;
import spring.di.ui.GridExamConsole;
import spring.di.ui.InlineExamConsole;

public class Program {

	public static void main(String[] args) {
		
		Exam exam = new NewlecExam();
		ExamConsole console = new InlineExamConsole(exam);
//		ExamConsole console = new GridExamConsole(exam);
		//Di인데 DI하는 것을 코드 수정이 아닌 외부 수정파일로 객체 생성부분만 전담해준다면 코드 수정없이 외부 수정파일 만 변경하고도 원하는 객체를 조립?의존성변경? 을 마음대로 할 수 잇따.
		
		
		console.print();
		
	}

}

```
* Exam (interface)
```
package spring.di.entity;

public interface Exam {
	int total();
	float avg();
}

```
* NewlecExam(Exam Interface를 구현한 클래스
```
package spring.di.entity;

public class NewlecExam implements Exam {
	
	private int kor;
	private int eng;
	private int math;
	private int com;
	
	
	@Override
	public int total() {
		// TODO Auto-generated method stub
		return kor+eng+math+com;
	}

	@Override
	public float avg() {
		// TODO Auto-generated method stub
		return total()/4.0f;
	}

}

```
* ExamConsole(Interface)
```
package spring.di.ui;

public interface ExamConsole {
	void print();
}

```
* InlineExamConsole( ExamConsole을 구현한 클래스)
```
package spring.di.ui;

import spring.di.entity.Exam;

public class InlineExamConsole implements ExamConsole {
	
	private Exam exam;
	
	public InlineExamConsole(Exam exam) {
		this.exam = exam;
	}



	@Override
	public void print() {
		System.out.printf("total is %d, avg is %f\n", exam.total(), exam.avg());
	}

}

```
* GridExamConsole( ExamConsole을 구현한 클래스)
```
package spring.di.ui;

import spring.di.entity.Exam;

public class GridExamConsole implements ExamConsole {

	private Exam exam;
	
	public GridExamConsole(Exam exam) {
		this.exam = exam;
	}
	
	@Override
	public void print() {
		System.out.println("┌─────────┬─────────┐");
		System.out.println("│  total  │   avg   │");
		System.out.println("├─────────┼─────────┤");
		System.out.printf("│   %3d   │  %3.2f   │\n",exam.total(),exam.avg());
		System.out.println("└─────────┴─────────┘");
	}
	
}

```

# 스프링 프레임워크 강의 6강 - 스프링 DI 설정을 위해 이클립스 플러그인 설치하기
* 이클립스에 Spring 플러그인 설치
   * Help 탭
   * Eclipse Marketplace
   * Find에 spring검색 
   * ![image](https://user-images.githubusercontent.com/40667871/218475139-7dc59aa2-e409-4a68-b192-eb6324794357.png)
   * install
   * confirm
   * accept and finish
* 설치한 tool은 spring을 사용하는데 꼭 필요한건 아니다. 설정파일 다 쓰기 귀찮아서
* spring plug in 을 이용해 spring 설정파일 생성
   * 파일생성
   * others
   * spring 폴더
   * spring bean configuration file 만들기
   * 생성된파일
   ```
   <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


</beans>

   ```
   
