* 실행할 때 class파일도 그렇고 컴파일 할 때 java파일도 그렇고 package 구조가 지켜져야한다. 또한 컴파일 자체도 패키지가 시작되는 루트디렉토리에서 해야한다!!

# 스프링 프레임워크 Part2 AOP 강좌 06강 - After Returning / Throwing 어드바이스 구현하기
* AroundAdvice/BeforeAdvice/AfterReturningAdvice/AfterThrowingAdvice
* ThrowAdvice는 상속할 메서드가 없음
* settings -logAfterReturningAdvice,logAfterThrowingAdvice 추가 후 proxy property에도 value로 추가
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.3.xsd">
	
	
	<bean id="target" class="spring.aop.entity.NewlecExam" p:kor="101" p:eng="1" p:math="1" p:com="1"/>
	<bean id="logAroundAdvice" class="spring.aop.advice.LogAroundAdvice"/>
	<bean id="logBeforeAdvice" class="spring.aop.advice.LogBeforeAdvice"/>
	<bean id="logAfterReturningAdvice" class="spring.aop.advice.LogAfterReturningAdvice"/>
	<bean id="logAfterThrowingAdvice" class="spring.aop.advice.LogAfterThrowingAdvice"/>
	<bean id="proxy" class="org.springframework.aop.framework.ProxyFactoryBean
	">
		<property name="target" ref="target"/>
		<property name="interceptorNames">
			<list>
				<value>logAroundAdvice</value>
				<value>logBeforeAdvice</value>
				<value>logAfterReturningAdvice</value>
				<value>logAfterThrowingAdvice</value>
			</list>
		</property>
	</bean>
		
</beans>

```
* AfterReturningAdive
```
package spring.aop.advice;

import java.lang.reflect.Method;

import org.springframework.aop.AfterReturningAdvice;

public class LogAfterReturningAdvice implements AfterReturningAdvice{

	@Override
	public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
		// core concern의 반환값을 갖고 어떤 처리를 할 경우 사용
		System.out.println("returnValue:"+returnValue + ", method:"+method.getName());
	}

}

```
* After Throwing
```
package spring.aop.advice;

import org.springframework.aop.ThrowsAdvice;

public class LogAfterThrowingAdvice implements ThrowsAdvice{
	
	//매개변수로 들어올 예외가 너무 많아서 다른것과 다르게 고정된 메서드가 없음
	//core-concern에서 예외가 발생할 때 실행되는 메서드
	public void afterThrowing(IllegalArgumentException e) throws Throwable{
		System.out.println("예외가 발생하였스니다.:"+ e.getMessage());
	}
}

```
* 총 실행결과 - 예외 없을 때
![image](https://user-images.githubusercontent.com/40667871/220109617-afd949ae-bb81-4b0b-bb7a-035bba428b96.png)
* 실행결과 - 예외 있을 때
![image](https://user-images.githubusercontent.com/40667871/220109721-ea412e1b-7b32-4fba-a7e4-824c30f182df.png)

# 스프링 프레임워크 Part2 AOP 강좌 07강 - Point Cut(Weaving, Join Point)
* spring aop구현시 필수 용어 : 포인트컷(Pointcuts)/조인포인트(JoinPoint)/위빙(weaving)
![image](https://user-images.githubusercontent.com/40667871/220110050-88cb2da3-3e60-4d60-a402-a58691536302.png)
* 마치 뜨개질 같음(weaving)
* joinoint core concern 중 조인하게 될 메서드
* 기본적으로 proxy는 target(core concern)의 모든 메서드를 join point로 생각
* Point cut : target중 원하는 메서드만 빼오는 것 Advice별로 사용가능
* Point cut 설정방법 1
```
<bean id="classicPointCut" class="org.springframework.aop.support.NameMatchMethodPointcut
">
		<property name="mappedName" value="total"/>
	</bean>
	
	<bean id="classicBeforeAdvisor" class="		org.springframework.aop.support.DefaultPointcutAdvisor
	">
		<property name="advice" ref="logBeforeAdvice"/>
		<property name="pointcut" ref="classicPointCut"/>
	</bean>
	
	<bean id="proxy" class="org.springframework.aop.framework.ProxyFactoryBean
	">
		<property name="target" ref="target"/>
		<property name="interceptorNames">
			<list>
				<value>logAroundAdvice</value>
				<value>classicBeforeAdvisor</value>
				<value>logAfterReturningAdvice</value>
				<value>logAfterThrowingAdvice</value>
			</list>
		</property>
	</bean>
```
* advice하나당 advisor 하나를 만들어야하는 번거로움이 있지만 더 간단한 방법 추후에 배움(지금 배운건 classical한 방법)

# 스프링 프레임워크 Part2 AOP 강좌 08강 - 간소화된 Advisor
* advisor와 pointcut을 합치는법 - advisor 중에 pointcut의 기능 내재화된 것 존재
* advisor와 pointcut 합친것 예시
```
<!-- <bean id="classicBeforeAdvisor" class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor
	">
		<property name="advice" ref="logBeforeAdvice"/>
		<property name="mappedNames">
			<list>
				<value>total</value>
				<value>avg</value>
			</list>
		</property>
	</bean> -->
	
	<bean id="classicBeforeAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor
	">
		<property name="advice" ref="logBeforeAdvice"/>
		<property name="patterns">
			<list>
				<value>.*to.*</value>
			</list>
		</property>
	</bean>
	
	<bean id="classicAroundAdvisor" class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor
	">
		<property name="advice" ref="logAroundAdvice"/>
		<property name="mappedName" value="total"/>
	</bean>
```

