# ECMA Script 6 / 2015 강의 21 - static 멤버 정의하기
* 멤버변수 = 인스턴스 속성/변수
* 객체마다 고유한 속성을 의미. 객체 마다의 고유한 값이며 공간
* 객체 공통된 값은? static값으로 = 클래스 변수
* static #info  로 만듦 - 전역변수처럼 동작
* 사용할 땐 static 함수를 통해 사용
* 함수 앞에도 static붙이면됨

# ECMA Script 6 / 2015 강의 22 - 속성 은닉화를 위한 쉬운 Getters/Setters 지원
* 캡슐 밖, 안 차이
![image](https://github.com/resti999/TIL/assets/40667871/e908d51c-3706-4a47-a8a4-d89c961be436)
* 함수로 은닉된 변수에 접근했다.
![image](https://github.com/resti999/TIL/assets/40667871/63c5be7b-ad5e-4bb6-933a-573e3a2b3614)
* 왜 private으로 숨기는가?  데이터 구조를 숨기기위해? 데이터 구조가 변화했을 경우 유연하게 대처하기 위함
* 구조가 달라진다 = 변수가 추가되거나 변경되거나 삭제되는 일
![image](https://github.com/resti999/TIL/assets/40667871/fcdaaec2-828d-419c-8a51-482eb2f09943)
* 구조가 변경됐을 때 그 변수를 사용한 모든 코드를 고쳐야하는 일 발생
![image](https://github.com/resti999/TIL/assets/40667871/920d8657-ad97-4e95-8405-eb6d723603f4)
* Getter/Setter를 사용하면 사용하는 쪽의 코드를 변경하지 않아도 될 수 있다.
![image](https://github.com/resti999/TIL/assets/40667871/01b43a42-538e-4bc0-9841-82eb983a9eb9)
* 사용할 땐 exam.kor=30 으로 가능하다.
* getter/setter을 사용하면  exam.kor++과 같은 연산도 동작한다.

# NodeJS 웹 프로그래밍 강의 01 - 노드JS(NodeJS)란 무엇인가?
* Node.js란?
* 자바 스크립트 실행 환경 = Web Browser
* V8 크롬에서 사용하는 Plug-in식 자바스크립트 실행 환경
* V8에 콘솔을 결합
![image](https://github.com/resti999/TIL/assets/40667871/1a7705e7-8266-400f-aa8c-863266f346f4)
![image](https://github.com/resti999/TIL/assets/40667871/b5e3047a-3cb7-4549-9cac-6d87bdd1ee39)
* nodejs 는 명령어를 입력해서 실행할 수도 있고 배치파일을 실행할 수도 있다.
![image](https://github.com/resti999/TIL/assets/40667871/45d3798f-0508-40c7-88a5-e44e21058263)
![image](https://github.com/resti999/TIL/assets/40667871/394af073-5fb6-4507-8c5b-02f61c57eead)
* js가 노드js로 인해 탈브라우저가 됨

# NodeJS 웹 프로그래밍 강의 02 - 개발환경 준비하기
# NodeJS 웹 프로그래밍 강의 03 - NodeJS의 역사
* js는 서버로 로그인을 할 때 서버로 전송될 데이터의 유효성을 검사하기 위해 만들어진 언어
![image](https://github.com/resti999/TIL/assets/40667871/d8bc46f3-1889-4eac-a261-080d313b6289)
![image](https://github.com/resti999/TIL/assets/40667871/4af5a1b4-856a-4690-85fb-b3a352aa347d)
* 옛날엔 v8과 흡사한 NetScape사에서 만든 SpiderMonkey라는 엔진이 있었고 브라우저 외에 NetScape사에서 만든 LiveWire라는 플랫폼이 있어서 여기서 js를 실행할 수 있었다.
* 구글을 브라우저를 기반으로 office(구글시트), email(gmail), map등 다양한 프로그램을 만듦 -> 새로운 자바 스크립트 엔진의 필요성을 느낌
* 크롬 웹스토어 - 크롬을 기반으로 실행되는 수많은 어플리케이션들 상점 -> v8엔진을 만들고 다른데서 쓸 수 있게 만듦
![image](https://github.com/resti999/TIL/assets/40667871/5f8c01a7-045d-4e69-ae20-0c86682546a7)
* npm - nodejs가 사용하는 패키지들 관리도구(라이브러리)
![image](https://github.com/resti999/TIL/assets/40667871/2063fad3-078f-41d5-aa9f-5121af084102)
* Node 10부터 ES6 조금씩 반영됨



