# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 36강 - 제일 큰방 꾸미기
![image](https://user-images.githubusercontent.com/40667871/236450333-2dfbfac0-2a43-428d-85e9-7557643630ba.png)
![image](https://user-images.githubusercontent.com/40667871/236450819-c499eb90-7d62-4253-9857-bae093e4b19c.png)
* 블럭을 만들 때 가장 범용적인 태그는 div태그
* 디자인 파일로부터 각 영역의 높이를 잰다
![image](https://user-images.githubusercontent.com/40667871/236452206-cc5f5630-4de8-4e94-a49f-f978b725d272.png)
* header 태그는 문서에서 한번만 등장하는 것이 아니고 section tag 밑에 하나씩 존재할 수 있다.
* main 태그는 문서에서 한번만 사용
* 방 중에서 가장 큰 방은 id를 무조건 부여해야함, 한번 밖에 등장하지 않으니까
* 박스의 높이를 한정하지 않는 경우는 **컨텐츠**의 높이를 따라간다
* 높이도 설정 안하고 컨텐트도 없으면 아예 표시가 안된다.
* 박스가 줄었을 때 컨텐트는 (overflow옵션)
   * 컨텐트가 넘치면 보여줄것인지
   * 스크롤해서 보여줄것인지
   * 넘친거 안보여줄것인지
* 방을 만들 때는 컨텐트는 주석을 해놓고 하는게 좋다. 
* 기본적인  html 태그가 가진 스타일이 있어서 리셋을 하는게 좋다.

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 37강 - reset 파일 추가와 개발자 도구
*  컨텐트 , 패딩, 보더, 마진
*  마진 : 다른 태그와의 거리
*  패딩 : 보더와 컨텐트 사이의 간격
* <link rel="stylesheet" href="../css/style.css" type="text/css">를 줄 때 항상 reset.css가 먼저 적용돼야 하기 때문에 맨 위에 나와야함.
![image](https://user-images.githubusercontent.com/40667871/236456134-761a879e-d118-4d58-a9b2-6bfecb90390d.png)
* 개발자 도구에 스타일 히스토리 나와있고 어떤파일에서 적용됐는지도 나옴
* css 외부 파일을 포함하는 방법 2가지 
   * link 태그 이용 
   * css파일에서  @import로 reset.css파일 가져오기 - ex) @import url("reset.css");
* 2번째 방법은 파일 총 로드시간이  1번째 방법보다 오래걸림, 두번에 걸쳐서 로드하기 때문, 관리하기는 2번째 방법이 편함

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 38강 - 색상 값
* HEX : 빛의 3원색 rgb   #ffffff
*  RGB decimal values : rgb(255,0,0);
![image](https://user-images.githubusercontent.com/40667871/236458922-eaa16d90-0b90-45d8-9349-c08a9bec8119.png)
* rgba : transparent 추가한것 1에 가까울 수록 불투명 - 모든 브라우저 호환되진 않음
* hex에서 알파(투명도) 적용하는 법 -> #ffffff10  맨 뒤에 두자리 사용 ff 가 완전 불투명
![image](https://user-images.githubusercontent.com/40667871/236459762-935b6070-7b3c-4bac-af91-dd407ef6c56f.png)
* 윈도우 테마에 따라서 색상 가져올 수 있음 - 많이 쓰진 않음
