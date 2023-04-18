# 프론트엔드, 백엔드 개발자를 위한 HTML, CSS 강의 06강 - Markup 이란?
* HyperText + MarkUp
* HyperText - 링크
* MarkUp - 링크인지 표시해줌, 문자를 감싸서 특정기능을 한다는걸 나타내는 표시 ->태그사용 <>
* html - hypertext markup language

# 프론트엔드, 백엔드 개발자를 위한 HTML, CSS 강의 07강 - HyperText 란?
* HyperText? 링크 -> markup중에서 <a>태그 사용 , a=anchor= 닻, hyper=beyond=너머

# 프론트엔드, 백엔드 개발자를 위한 HTML, CSS 강의 08강 - HTML파일의 기본구조
* 기본구조 : 규칙(문법) - 의사전달을 위함
```
<html>
   <head> -title 등 메타정보
   </head>
   <body>
   </body>
</html
```
  
# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 09강 - DOCTYPE과 W3C
* 브라우저가 여러개 있어서 똑같은 태그라도 브라우저마다 렌더링하는 방식이 달랐고 각 브라우저마다 각자의 태그를 만드는 일 발생. 통일의 필요성 생김
* W3C : 브라우저와 태그의 통일성을 위해 생긴 기구(MIT LCS, ERCIM, 일본게이오대학)
* W3C의결 절차
![image](https://user-images.githubusercontent.com/40667871/232808358-36159512-f7a1-480e-a76f-ab0025a639e4.png)
* 브라우저회사들은 권고안대로 브라우저를 구현, 강제성은 없다
* draft 는 초안
* DOCTYPE : W3C에서 발표한 권고안
![image](https://user-images.githubusercontent.com/40667871/232808847-d2e38ac2-42a2-482e-9696-6c5c59f9d584.png)
* 어느 권고안도 따르지 않고 자기 브라우저만의 렌더링방식 사용 : Quirks
* HTML 5 부터는 <!DOCTYPE html> 하나만 존재 -> w3c의 렌더링 방식 따를지말지만 결정!
