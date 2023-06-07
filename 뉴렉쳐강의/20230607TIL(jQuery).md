# 제이쿼리(jQuery) 프로그래밍 1강 - jQuery란 무엇이며 우리는 무엇을 배우게 될 것인가?
* jquery가 무엇이고 필요성, 썼을 때 이점, 안썼을 때 불이익 학습
* jquery 의 장점
   * 가장중요한장점: Cross browsing - 브라우저들 간 문서객체 dom의 표준이 없었는데 (지금은 w3c가 표준냄) 단일하게 개발할 수 있는 환경 제공해줬다.
   * dom의 노드객체를 선택하는 방법이 어려웠는데 css selector를 이용해 노드객체를 선택할 수 있게 해줌(지금은 표준으로 제공하는듯??)
   * 32kb의 가벼운 라이브러리
* 요즘은 w3c의 표준을 따라가는 추세라 저절로 브라우저가 cross-browsing이 지원. css selector 가  selectors API가 표준으로 제공됨.
* 웹표준이 jquery를 배끼는 중
* 아직까진 jquery를 많이 쓰기 때문에 알아두면 좋다.
* jquery는 api를 보면서 해도 충분하다. 강의들을 필요 x
* jquery 동작방식 - 각 브라우저의 Dom 구조가 다른것을 숨기고 jquery object로 만들어 그걸이용하게함
![image](https://github.com/resti999/TIL/assets/40667871/e34abe10-f83b-486a-8109-b1d3fa02a1e8)
* jquery는 언어가 아닌 플랫폼이다. js 가 언어

# 제이쿼리(jQuery) 프로그래밍 2강 - jQuery로 시작하기
* js파일 compressed(압축본)의 의미 : 알집처럼 압축됐다는것이 아니고  빈공백(white space)가 없다는 뜻. 한줄로 쭉 이어지게 만든 코드 + 사용자가 만든 변수명을 짧게 s등 임의로 줄임. 배포할 때만 사용 개발할 때는 비추
* CDN(Content Delivery Network) : 라이브러리를 온라인에서 호스팅해주는 것. 로컬에 다운받아 놓을 필요가 없다.
   * cdn을 사용하는게 캐시로 인한 효율성있음
   * jqeury lib는 유명해서 cdn 주소로 다른데서 받아서 캐싱됐을 확률이 높다. 내 로컬에서 제공하면 클라이언트에서 다운을 받아야 하기 때문에 불필요하게 다운을 받는 일이 발생. 비효율적 
* **jQuery는 dom을 대신하는 lib**
![image](https://github.com/resti999/TIL/assets/40667871/d81d02c4-1e25-4b4a-93f5-07f58a1f9626)
* 기존 노드객체를 jquery객체로 박싱하는 작업 선행
![image](https://github.com/resti999/TIL/assets/40667871/25bab3b7-3d42-4854-83fc-b63bcad09486)
* 기존 각 브라우저의 dom을 조작하는게 아니라 jQuery lib을 이용, 함수도 다 jQuery 만의 이름이 있다.(기존 브라우저들과 다름)
* jQuery(img1).src = "images/img1.jpg"; 이렇게 할 경우 src는 없는 속성이지만 에러가 안난다 왜?  js특성상 src라는  속성을 만들고 문자열을 대입하기 때문에 
* js에서 변수,함수명을 만들 때 특수기호를 쓸 수 있지만 $는 유일하게 사용가능  ->  jQuery(함수)자체를 $로 치환
* jquery는 dom의 기능을 안쓰기 위해만들어졋는데 dom의 기능을 불러와서 포장하면 이상하다 -> 노드를 선택해서 가져오는걸 내장하고 있다.
* jquery에 dom 객체를 넣으면 jquery객체로 변환해주는 기능도 하지만 - $(dom객체)   ,  $("img1") css selector 문자열로 가능 ->**문서에서 노드를 찾는 작업까지 해줌**

# 
