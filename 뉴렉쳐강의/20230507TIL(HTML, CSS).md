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
![image](https://user-images.githubusercontent.com/40667871/236659323-b290da08-028a-4788-9677-930c257e2c06.png)
* 컨텐트 = 텍스트
* 3.0 에 flex, grid가 등장
![image](https://user-images.githubusercontent.com/40667871/236659382-158944a8-d643-4fc1-9a80-6678b83645b2.png)
![image](https://user-images.githubusercontent.com/40667871/236659401-e42d327f-4732-4de2-84cc-9e8d90741d3c.png)\
* 1,2를 포함하는 박스의 속성을 display:flex , 자손들은 flex item이 돼서 속성에 따라 배치됨
* main 축과 cross축으로 모든걸 나눔
![image](https://user-images.githubusercontent.com/40667871/236659445-65d21869-0d3e-49e3-8b5f-9d6d793c81b4.png)
![image](https://user-images.githubusercontent.com/40667871/236659487-9ee8e9e3-a242-4e4f-9470-4607e46d62e5.png)
![image](https://user-images.githubusercontent.com/40667871/236659494-12dcbdec-a224-4b20-8172-3737d3a546c5.png)
![image](https://user-images.githubusercontent.com/40667871/236659503-60d27bdf-58c2-4a7b-b0c5-3c7581c92435.png)
![image](https://user-images.githubusercontent.com/40667871/236659505-5361bf25-c974-4ef1-9e6f-b371c34b4563.png)



# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 46강 - Flex Direction / Basis
![image](https://user-images.githubusercontent.com/40667871/236659746-16de7769-09cb-43c6-a4c6-aef2f34d0280.png)
![image](https://user-images.githubusercontent.com/40667871/236660503-c81dfab0-dba6-4287-8bfb-7659dc7a34e5.png)
* flex 와 inline-flex 의 차이 : flex box 안의 태그들이 inline 태그들이냐 box 태그들이냐의 차이, 개념상으로는 이렇지만 사용하는 방법은 동일
* flex box의 등장으로 박스들을 오른쪽으로 정렬할 수 있게 됐다.
* flex-direction:flex-box에서 설정, column 으로 하면 원래처럼 수직으로 정렬   row가 defalut고 수평정렬.  row-reverse는 오른쪽 정렬
* flex item 의 넓이 default는 컨텐츠의 크기만큼 크기를 가짐. 높이도 컨텐트 크기만큼
* flex item 의 css속성 중 width를 지정해도 넓이 지정가능 -방향성에 상관없이 너비, 높이 정할 때
*  flex item의 축 방향 별 크기를 정하는 것  flex item 에서 flex-basis 속성 설정

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 47강 - flex-grow
* flex box의 빈 영역 없이 item 들로 꽉 채우고 싶을 때 아이템들에 flex-grow:1  사용
* flex-grow:1  : item 들이 차지하고  남은 **빈 공백**을 각 아이템이 차지하는 비율
* 부여받은 여백의 크기가 3배라는게 중요. 컨테츠의 크기가 3배라는 것이 아님
* flex box에 적용하는 속성과 flex item에 부여하는 속성 구분 잘해야함
* 한 item 만 flex-grow 속성을 가지면 속성값이 무엇이든 그 item만 여백을 가진다. 속성값은 의미가 없다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 48강 - flex-shrink
* 




