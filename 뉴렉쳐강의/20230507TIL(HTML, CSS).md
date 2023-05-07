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
* frex-grow와 반대 되는 속성?
* 공간이 줄어들면? item 의 기본크기보다 줄어들면? 
* 공간이 줄었는데 줄이고 싶지 않다 - flex-shrink : 0 으로 설정 ->오버플로
* shrink값이 1(줄어드는것으로 설정)이면 컨텐트 크기까지 줄어듬
* grow는 설정하지 않으면 0(여백차지x)  , shrink는 설정하지 않으면 1(줄어듬), basis 설정하지 않으면 컨텐트 크기

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 49강 - flex-grow/flex-shrink/flex-basis의 단축 속성과 값
![image](https://user-images.githubusercontent.com/40667871/236663212-e9b2b49b-8688-417c-ba9c-7537b8fb8837.png)
* mdn 등의 docs,reference를 잘 활용하자.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 50강 - flex-lines(drection,wrap,flow)
![image](https://user-images.githubusercontent.com/40667871/236663934-c1a478f6-612d-4449-a9dd-b01ba8ada51b.png)
![image](https://user-images.githubusercontent.com/40667871/236663943-0c6b7070-e8dc-4864-aebc-86706083749c.png)
![image](https://user-images.githubusercontent.com/40667871/236664190-21f6f1dd-6f68-4570-8e5f-9985901531e1.png)
* flex-wrap: wrap :   각 item의 크기보다 브라우저창이 작아지면 나머지 item들은 밑으로 내려가게 설정
* 슬라이딩 설정 할 때 nowrap 설정
* direction이 column일 땐 wrap안먹지만 부모 컨테이너가 높이가 설정될 경우는 wrap먹음
* 수직 방향에서 래핑에서 여백이 생기면? grow옵션을 줘야함 
![image](https://user-images.githubusercontent.com/40667871/236664258-2ca42982-acbf-47ec-b823-ecbc084a7233.png)
* flex-flow 속성은  direction과 wrap을 한번에 표현할 수 있다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 51강 - Ordering
![image](https://user-images.githubusercontent.com/40667871/236665449-2ab7cf9e-ef6e-4504-b90b-f4b38aa2a6ad.png)
* 기본 순서는 태그순서
![image](https://user-images.githubusercontent.com/40667871/236665471-64635dae-5471-44b5-aec4-290af80345bc.png)
* order 숫자 작을 수록 처음에 나옴 direction에 따라 다름
* 특정한 것들끼리 위치를 바꾸려면 전체다 번호를 매겨야한다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 52강 - Box Alignment #1 : justify-content
![image](https://user-images.githubusercontent.com/40667871/236665996-4104402c-54d7-4fed-a48a-612ff30e86fd.png)
* justify-contetnt : main axis
* align-items : cross axis
![image](https://user-images.githubusercontent.com/40667871/236666061-be6b87d1-4f83-41cf-9d4a-47051a0cf0f2.png)
* 수직방향에서는 여백이 없으므로 container 에 높이를 반드시 설정해야만 한다.
* 여백이 없으면 정렬할 필요가 없다.
* justify-content : space-around : 컨텐트 수 X2만큼 여백이 나눠짐 컨텐트 양 옆에 여백 위치

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의, 인강 53강 - align-items/align-content/align-self
![image](https://user-images.githubusercontent.com/40667871/236667160-9e11386f-2a25-45fe-8e11-e67b0b3b3bd3.png)
![image](https://user-images.githubusercontent.com/40667871/236667202-a361fbcc-0f79-420f-ae37-36763d3db1c0.png)
* baseline : 컨텐트를 기준으로 정렬 박스크기 상관없이, 컨텐트의 밑줄을 맞춤
* stretch : 부모 높이만큼 꽉채움
* align=self : 각 아이템의 위와같은 cross-axis 정렬옵션 줄 때 사용
![image](https://user-images.githubusercontent.com/40667871/236667443-9fb6e588-8982-4552-9f5f-7c03f782e8de.png)
* align-items 의 기본 값은 stretch
![image](https://user-images.githubusercontent.com/40667871/236668834-7cdea2c0-75fb-415e-b479-0a8addb649d5.png)
* align-content는 align-items 와는 다르게 한 줄을 통째로 패킹해서 같이 움직임
* flex box 의 라인? wrap 을 통해서 층 수가 늘어 나 때 한 층을 한 라인이라고 한다.
* align-items 를 적용하면 위아래르 꽉채워서 부모크기만큼 차지하던게 컨텐트 크기로 바뀌고 정렬됨
* align-items와 align-content의 차이점: content가 층을 무시하고 모든 층을 한번에 정렬함, content 는 여백을 사이에 꽂아넣는 space옵션이 있음, items 는 자체적으로 나누기 때문에 없음
* align-items:center
![image](https://user-images.githubusercontent.com/40667871/236669361-e51238c7-8ad2-4154-aec4-657bd69d3b59.png)
* align-content:center
![image](https://user-images.githubusercontent.com/40667871/236669348-bec43fc5-0c81-4170-b779-03ab2d8f84d6.png)


