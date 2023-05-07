# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 41강 - 콘텐츠 블록과 절대좌표
* css 2.0 레이아웃 지원안했었음 . 3.0에서  레이아웃 기법생김   3.0에선 어플리케이션으로 바라봄
![image](https://user-images.githubusercontent.com/40667871/236654792-2ceda72c-c019-4c7d-98da-4b6ea89d6d3e.png)
*  grid는 이제 호환중이라 호환되는지 문제가 있음
* static기법의 포지셔닝 기법(옛날 2.0)
![image](https://user-images.githubusercontent.com/40667871/236654824-b1e66287-d0a0-43c3-8cfa-8e2010720fe5.png)
* css가상 클래스 선택자  : 세미콜론이 들어감
* 기본 postition 속성은 static - 정해진 자리에 가있으라는 뜻
* position: absolute - 태그의 위치를 특정한 위치로 보냄 ,부모가 없어진 것과 같음, width100%가 사라지면서 컨텐트의 영역만 차지하게 된다.
* 원래 태그는 나중에 나온게 전에 나온 태그를 가리는데 absolute속성을 지니면 최상단에 위치
* absolute의 기준좌표는? 문서좌표
* 부모를 기준으로 포지셔닝하는 방법은? 부모 태그 css속성에 position:relative 후 자식 태그에 position:absulute하면됨  -> 부모안에 포함이 되게 되기 때문에 width 100%를 자식태그에 적용할 경우 부모의 크기만큼 차지하게 된다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 42강 - 상대위치(position:relative)
![image](https://user-images.githubusercontent.com/40667871/236655561-a65c2c16-2672-4eb0-b05c-212bae654f79.png)
* static이 아니면 left와 top속성은 의미를 갖는다.
* relative : static 위치에서 ~만큼 이동
* relative 의 특징  sibling들이 자기 위치 차지 못하게 함, 움직여도 기존 자기 자리는 그대로. 그리고 기존에 차지하던 width 그대로 위치만이동
![image](https://user-images.githubusercontent.com/40667871/236655617-0d6dc440-fc7b-4aa5-9bd6-e8a89af60e71.png)
* 부모가 relative면 자식이 absolute여도 그 안을 기준으로 포지셔닝되는게 중요.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 43강 - 고정위치(position:fixed)
* fix : absolute와 같이 sibling에 자기 자리 내주고 , 넓이도 컨텐츠 넓이만큼 쪼그라듬
* fixed vs absolute 차이 : fixed는 스크롤바를 내려도 그대로, 위치가 화면에 고정됨 문서에 고정되는게 아니라 - 팝업이나 그런거에 사용하면 좋겠네? 도구바, 퀵메뉴 등등
* absolute같은 경우는 offset영역이 존재하지만 fixed는 부모가 relative라 해도 어떤 곳에도 종속되지 않고 화면크기를 기준으로만 위치한다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 44강 - 붙임위치(position:sticky)
* sticky는 fixed와 비슷  , 최근해 등장했고 모바일용
* fixed 의 문제 - fixed의 자리를 동생들이 차지(내용가림), 그리고 컨텐츠 영역만큼만 차지로 바뀜
* 위 문제 해결 가능한 것이 sticky, 꼭 top을 줘야함. 내위치는 뺏기지 않으면서 특정 화면 위치 고수 가능

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 45강 - Flex 레이아웃과 용어
* 
