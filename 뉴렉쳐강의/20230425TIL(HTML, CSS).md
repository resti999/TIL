# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 17강 - 웹문서 작성 1단계-콘텐츠 작성하기
* 비트맵파일(이미지파일) 받음
* 컨텐트가 무엇인지 파악
* 크롬 확장프로그램중 web developer - css뺀 html 컨텐츠를 보여줌
![image](https://user-images.githubusercontent.com/40667871/234303524-7cc0e031-c11c-442e-bc3b-e0a88d3c3bce.png)

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 18강 - 콘텐츠의 블록 작성하기 #1
* 비트맵을 받고 탑다운 방식으로 방을 나눈 후에 하지만 경험이 필요(거시적인 것부터)
* 경험이 없을 때는 다운투탑으로 컨테츠부터 적기 (미시적으로 접근)
![image](https://user-images.githubusercontent.com/40667871/234307521-381b9259-3038-46c3-8aa7-c3f2df8c0439.png)
![image](https://user-images.githubusercontent.com/40667871/234308915-033a8ea8-342d-49ba-9421-b0e30b1c4558.png)
![image](https://user-images.githubusercontent.com/40667871/234309340-14060d9e-6f6a-4d78-9d2e-a3199365353d.png)

# 프론트엔드, 백엔드 개발자를 위한 HTML5, CSS3 강의 19강 - 콘텐츠의 블록 작성하기 #2
* 컨텐츠가 5가지 범주에 속하는지 파악 : 제목, 목록, 문장, 표, 폼  에 해당하지 않으면 div로 분류
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <a href="../index.html">홈</a>
    <a href="list.html">공지사항목록</a>
    <a href="../member/login.html">로그인</a>
    <a href="../member/signup-agree.html">회원가입</a>
    <h1>공지사항 목록 페이지</h1>

    <h1>뉴렉처 온라인</h1>

    <ul>
        <li>학습가이드</li>
        <li>강좌선택</li>
        <li>AnserIs</li>
    </ul>

    <form action="">
        <fieldset>
            <legend>검색입력필드</legend>
            <label for="">과정검색</label><input type="text"><input type="submit" value="검색">
        </fieldset>
        
    </form>

    <ul>
        <li>HOME</li>
        <li>로그인</li>
        <li>회원가입</li>
    </ul>

    <ul>
        <li>마이페이지</li>
        <li>고객센터</li>
    </ul>

    <h2>고객센터</h2>

    <h3>고객센터메뉴</h3>

    <ul>
        <li>공지사항</li>
        <li>자주하는 질문</li>
        <li>수강문의</li>
        <li>이벤트</li>
    </ul>

    <h3>협력업체</h3>

    <ul>
        <li></li>
        <li></li>
    </ul>

    <h2>공지사항</h2>

    <ol>
        <li>home</li>
        <li>고객센터</li>
        <li>공지사항</li>
    </ol>

    <table border="1">
        <tr>
            <td>번호</td>
            <td>제목</td>
            <td>작성자</td>
            <td>작성일</td>
            <td>조회수</td>
        </tr>
        <tr>
            <td>3</td>
            <td>자바 백엔드 개발자들을 위한 새로운 강의를 녹음 중에 있습니다.</td>
            <td>newlec</td>
            <td>2019-02-18</td>
            <td>169</td>
        </tr>
        <tr>
            <td>2</td>
            <td>유투브에 jQuery와 Angular 프레임워크 1강이 등록되었습니다.</td>
            <td>newlec</td>
            <td>2019-02-18</td>
            <td>1119</td>
        </tr>
        <tr>
            <td>1</td>
            <td>앞으로 모든 강의를 무료로 새로 시작합니다.</td>
            <td>newlec</td>
            <td>2018-11-18</td>
            <td>685</td>
        </tr>
    </table>

    <div>
        1 / 1 pages
    </div>

    <div>
        <div>이전</div>
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
        </ul>
        <div>다음</div>
    </div>

    <h2>회사정보</h2>

    <dl>
        <dt>주소:</dt>
        <dd>서울특별시 마포구 토정로35길 11, 인우빌딩 5층 266호</dd>
        <dt>관리자메일:</dt>
        <dd>admin@newlecture.com</dd>
        <dt>사업자 등록번호:</dt>
        <dd>132-18-46763</dd>
        <dt>통신 판매업:</dt>
        <dd>신고제 2013-서울강동-0969 호</dd>
        <dt>상호:</dt>
        <dd>뉴렉처</dd>
        <dt>대표:</dt>
        <dd>박용우</dd>
        <dt>전화번호:</dt>
        <dd>070-42026-4084</dd>
    </dl>

    <div>Copyrigt c newlecture.com 2012-2014 All Right Reserved. Contact admin@newlecture.com</div>
</body>
</html>
```
