# Spring MVC (스프링 웹 MVC) 강의방향 안내
* 서블릿을 쓸 때 쓰는 library는 tomcat의 web.xml에 설정했다. Spring Dispatcher도 쓴다.
* Dispatcher =프론트 controller
* spring boot - 복잡한 설정을 한번에 설정하게 해줌( application.properties or YAML)

# Spring MVC (스프링 웹 MVC) 강의 01 - Spring MVC란
* spring이 mvc를 지원한다?
* 서블릿을 할 때 모든 컨트롤러에 dispacher-(servlet중하나)로 forwarding을 했음
![image](https://user-images.githubusercontent.com/40667871/220619255-927527d7-83fe-4856-a68d-6c6dc8915ded.png)
* dIspatcher는 하나만
![image](https://user-images.githubusercontent.com/40667871/220619973-859ac5b5-149c-4af3-8b9b-c0b3dca7b8b7.png)

# Spring MVC (스프링 웹 MVC) 강의 02 - 실습환경 준비하기
* springtool4suite down.  spring.io
* maven 프로젝트 생성
* maven에서 라이브러리 추가는 pom.xml에서 dependency 추가, tomcat api dependency추가
* Could not initialize classorg.apache.maven.plugin.war.util.WebappStructureSerializer 해결법, pom.xml에 추가
```
<build>
	<plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-war-plugin</artifactId>
      <version>3.2.2</version>
    </plugin>
	</plugins>
</build>
```
# Spring MVC (스프링 웹 MVC) 강의 03 - 메이븐을 이용한 기본 웹 프로젝트 생성하기


# Spring MVC (스프링 웹 MVC) 강의 04 - Dispatcher Servlet 라이브러리 설정하기
* 메이븐 프로젝트에 스프링을 얹는다?
* 프론트 컨트롤러
![image](https://user-images.githubusercontent.com/40667871/220641628-12e626ec-e2af-4477-8c49-18e24d24263f.png)
* 메이븐을 이용해 스프링 라이브러리 다운
* 메이븐 라이브러리는 로컬 저장소. .m2에 저장
* spring web mvc dependency 추가
* 다음 강의에서 front controller인 dispatcher 서블릿 설정 안내
