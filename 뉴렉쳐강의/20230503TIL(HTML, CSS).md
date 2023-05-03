# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 28강 - 기본 선택자
* 특정 태그에 한번에 스타일을 적용할 때 선택자 이용
![image](https://user-images.githubusercontent.com/40667871/235939483-6ff01d05-cd70-4ede-9be4-3f5de172d0c3.png)
* 클래스 선택자는 앞에 .     ID일 경우는 # 붙여줘야함

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 29강 - 콤비네이션 연산자
* 콤비네이션 연산자 : 선택자를 어떻게 엮어서 사용하는지
![image](https://user-images.githubusercontent.com/40667871/235941297-073bb501-5030-4a03-88c9-064b9847f885.png)
* A B A 자손들 중에서 B태그를 찾아라
* A > B A의 자식(직계자손)들 중에서 B를 찾아라
* A + B A같은 수준 바로다음에 나오는 B를 찾아라
* A + B A같은 수준의 다음에 나오는 것들 중에서 B를 찾아라
![image](https://user-images.githubusercontent.com/40667871/235952035-bee89caf-8224-43b3-bb6a-12bdfa155a4f.png)
* 콘텐츠 블록은 가구 레이아웃은 방
* 각 콘텐츠 블록을 대표하는 태그에는 id를 쓸 수 있다. 
* css, html 에서 단어 구분자는 -

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 30강 - Sibling 연산자
* id는 문서내에서 유일하기 때문에 유일한 곳에만 사용
* li.first - 리스트태그 중 클래스 이름이 first인 태그
```
 <style>
        
        #main-menu>ul>li.first li{
            font-size: 30px;
            font-weight: bold;
        }
    </style>



<nav id="main-menu">
                <h1>메인메뉴</h1>
                <ul>
                    <li class="first">
                        <a href="">학습가이드</a>
                        <ul>
                            <li class="first">서브메뉴</li>
                        </ul>
                    </li>
                    <li><a href="" class="first">강좌선택</a></li>
                    <li><a href="">AnserIs</a></li>
                </ul>
            </nav>
```
* 스타일링은 컨텐츠 블록 단위로
