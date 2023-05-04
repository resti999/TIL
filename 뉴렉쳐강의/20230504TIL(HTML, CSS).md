# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 31강 - 속성 선택자
![image](https://user-images.githubusercontent.com/40667871/236222135-1802e944-d208-4016-98e5-9af1ac57d70b.png)
* 태그[속성="값"]  - 속성선택자    , =말고 다른 기호들이 많음.
* ~=  :공백으로 구분된 항목 중에서 하나 있으면 선택
* |="zh" :  -앞에 zh인 것 선택
* ^="#"  :  #으로 시작하는것 선택
* $=".org"  : .org로 끝나는것 선택
* \*="example" : example 이 포함되는 것 선택
* \*="insensitive" i : 위와 같으나  소대문자 안가림
* * \*="cAsE" s : 위위와 같으나 소대문자 가림

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 32강 - Selectors 우선 순위
![image](https://user-images.githubusercontent.com/40667871/236234887-998602ff-1988-4a05-b135-7b105729d588.png)
* 선택자 우선순위 : id>class>tag
* **적용범위가 넓은 선택자일수록 우선순위가 낮다.
* 동일한 연산 순위를 재정의하면 마지막 설정 적용
* 복합 연산자의 연산순위
![image](https://user-images.githubusercontent.com/40667871/236235939-becf750f-6ae7-4ab8-ba75-9046c44a6703.png)
* h1.h1 vs section > h1 - 콤비네이션연산자 사용한게 더 높은 우선순위를 가짐
![image](https://user-images.githubusercontent.com/40667871/236236725-f0e81dab-a2c9-4faf-9a5d-fdd2a3a1e397.png)
* 오렌지가 이김
![image](https://user-images.githubusercontent.com/40667871/236236811-137eee4b-fa13-4150-bff4-937bd51a5247.png)
* 클래스 vs 태그속성명 : 태그속성명이 클래스보다 우선순위 높음

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 33강 - 스타일 링크하기
* 스타일 외부 파일에 두는 이유 : 여러 페이지에 동일한 css가 같은데 수정을 하려면 여러 페이지에서 각각 해야하기 때문에  - 공통 분모는 하나의 파일로 관리해서 사용만 한다. 코드 집중화, 재사용성
* 외부 파일 참조 <link rel="stylesheet" href="../css/style.css" type="text/css">
* css파일 안에는 style태그 안에 있던 내용 적기
```
예를들면

#main-menu>ul>li.first~.aa{
    font-size: 30px;
    font-weight: bold;
}
```

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 34강 - 스타일 리셋, 평준화, 현대화
* reset.css : html의 기본 스타일이 있음 -> 마음에 안들어서 새로운 스타일을 입힐 것 -> html스타일 다 없애는것
* normalize.css : 모든 브라우저에 기본스타일을 통일
* 둘 중 하나 선택. -> reset.css를 많이 쓴다.
* 구버전의 브라우저 지원문제    html5를 지원하지 않는 브라우저
* HTML5SHiv : 브라우저가 인식하지 못하는 태그라도 document.createElement("article")과 같이 자바스크립트를 이용하면 인식을 하더라 에서 생긴 라이브러리
* Check Enabled : canvas , video등의 태그 를 사용 할 수 있는지 없는지 확인 해줌 - modernizr사이트에서 확인가능

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 35강 - 레이아웃 블록
* 우선 첫번재는 구조화된 html 문서를 만드는 것
* 각 콘텐츠 블록에 스타일을 입힘
![image](https://user-images.githubusercontent.com/40667871/236250632-b55a7842-2aac-4edc-8c33-46948eb0ecd0.png)
* 콘텐츠는 가구 레이아웃은 방
* 가구먼저? 방먼저? 보통 방먼저(레이아웃먼저)
* 서로 상호관계에 있는 속성들을 만들어가면서 배우는것이 도움이 된다.
* 레이아웃이란? 예전에는 table 로 배치했음
![image](https://user-images.githubusercontent.com/40667871/236251404-8bdd9286-44f9-461e-a660-c465c992bc27.png)
* 시각적으로 보이는 방
* 한 방향으로만 방을 만들어서 여러번 중첩해서 방을 나눈다.
![image](https://user-images.githubusercontent.com/40667871/236251816-21df0041-3ce6-429d-b909-1ad78de513e0.png)
![image](https://user-images.githubusercontent.com/40667871/236253735-b91b3115-1555-4e8e-a15b-aeca17b18c4c.png)
* css 2.0의 static,  css3.0 의 flex방식이 한방향으로 방을 나누는 css 기법들
* grid는 격자형 레이아웃기법인데 아직 호환성 문제가 있음
* 모델이 되는 사이트에 html 짜고 방을 생각해서 만들어보자 , 

