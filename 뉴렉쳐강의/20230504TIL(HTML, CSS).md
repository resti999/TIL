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
* 
