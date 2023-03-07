# Spring MVC (스프링 웹 MVC) 강의 40 - POST 입력 #2 (콤보박스 값 입력)
* response body에서 인코딩 설정을 했으니 한글 깨지는건 입력때 잘못 전달한것
```
<mvc:annotation-driven>
		<mvc:message-converters> <!-- @ResponseBody로 String 처리할때 한글처리 -->
			<bean class="org.springframework.http.converter.StringHttpMessageConverter">
				<property name="supportedMediaTypes">
					<list>
						<value>text/html;charset=UTF-8</value>
					</list>
				</property>
			</bean>
		</mvc:message-converters>
	</mvc:annotation-driven>
```
* 콤보박스 html 내용 : option안에 value를 지정해주지 않으면  카테고리1이 전달되고 value를 지정하면 value값이 전달된다.
```
<tr>
                                    <th>카테고리</th>
                                    <td class="text-align-left text-indent text-strong text-orange" colspan="3">
                                        <select name="category">
                                        	<option value="1">카테고리1</option>
                                        	<option value="2">카테고리2</option>
                                        	<option value="3">카테고리3</option>
                                        	<option value="4">카테고리4</option>
                                        </select>
                                    </td>
                                </tr>
```
* NoticeController내용
```
@ResponseBody
	public String reg(String title, String content, String category) {
		return String.format("title:%s<br> content:%s<br>category:%s", title, content,category);
	}
```

# Spring MVC (스프링 웹 MVC) 강의 41 - POST 입력 #3 (체크박스, 라디오버튼 입력)
* html 의 name속성은 값을 구분하는 key역할을 한다.
* 체크박스 보낼 때 같은 name으로 보내면 배열형태로 받아야 한다.
* 라디오버튼은 하나만 선택, 체크박스는 복수선택가능 - 일반적인 textbox도 name이 같은 2개의 값이 오면 배열로 받을 수 있다.
* 체크박스 html
```
<tr>
                                    <th>좋아하는 음식</th>
                                    <td class="text-align-left text-indent text-strong text-orange" colspan="3">
                                        <input type="checkbox" name="foods" value="1" id="ch1"><label for="ch1">짜장면</label>
                                        <input type="checkbox" name="foods" value="2" id="ch2"><label for="ch2">짬뽕</label>
                                        <input type="checkbox" name="foods" value="3" id="ch3"><label for="ch3">볶음밥</label>
                                        <input type="checkbox" name="foods" value="4" id="ch4"><label for="ch4">탕수육</label>
                                    </td>
                                </tr>
                                <tr>
                                    <th>가장좋아하는 음식</th>
                                    <td class="text-align-left text-indent text-strong text-orange" colspan="3">
                                        <input type="radio" name="food" value="1" id="ch1"><label for="ch1">짜장면</label>
                                        <input type="radio" name="food" value="2" id="ch2"><label for="ch2">짬뽕</label>
                                        <input type="radio" name="food" value="3" id="ch3"><label for="ch3">볶음밥</label>
                                        <input type="radio" name="food" value="4" id="ch4"><label for="ch4">탕수육</label>
                                    </td>
                                </tr>
```
* NoticeController 코드
```
@RequestMapping("reg")
	@ResponseBody
	public String reg(String title, String content, String category, String[] foods, String food) {
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return String.format("title:%s<br> content:%s<br>category:%s<br>food:%s", title, content,category,food);
	}
```

# Spring MVC (스프링 웹 MVC) 강의 42 - POST 입력 #4 (한글 입력이 깨지는 문제 )
* 데이터 인코딩 방식
![image](https://user-images.githubusercontent.com/40667871/223432650-bd551284-29fb-4302-930e-456e5a0c3bba.png)
* request encoding방식 변경 : 1. 톰캣 자체 설정 2.서블릿마다 설정
* 서블릿마다 request .setCharacterEncoding("UTF-8")설정하기 번거롭다 -> **필터**를 이용
* 서블릿 필터 : 여러개 사용 가능
* 서블릿 필터 사용예 : 인코딩, 권한 인증
![image](https://user-images.githubusercontent.com/40667871/223433233-65debd39-5f5c-4554-a69e-74f2fd121990.png)
* spring 필터 설정 **web.xml**에 설정
```
<filter>
		<filter-name>charaterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>charaterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

# Spring MVC (스프링 웹 MVC) 강의 43 - POST 입력 #5(파일 업로드 설정하기)
* 기본인코딩 데이터 전달 방식
![image](https://user-images.githubusercontent.com/40667871/223436373-ccb66805-4e68-47e5-b01f-9afcbc4c3553.png)
* binary 데이터 전달 방식 : multipart/form-data
![image](https://user-images.githubusercontent.com/40667871/223436590-c0fe8b4e-4637-42fc-bf10-54e448516083.png)
* 서버에서 multipart/form-data 방식으로 데이터 받는 설정(servlet-context.xml)
```
<bean id="multipartResolver"
		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<property name="maxUploadSize" value="314572800" />
	</bean>
```
* html form tag에서도 인코딩 방식 바꿔줘야함 : <form action="reg" method="post" enctype="multipart/form-data">
* apache가 제공하는 file lib도 있어야한다. pom.xml
```
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```
* file 선택 html
```
<tr>
    <th>첨부파일</th>
    <td colspan="3" class="text-align-left text-indent"><input type="file"
	    name="file" /> </td>
</tr>
```
* NoticeConroller 코드
```
@RequestMapping("reg")
	@ResponseBody
	public String reg(String title, String content,MultipartFile file, String category, String[] foods, String food) {
		
		long size = file.getSize();
		String fileName = file.getOriginalFilename();
		System.out.printf("fileName:%s, fileSize:%d\n", fileName, size);
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return String.format("title:%s<br> content:%s<br>category:%s<br>food:%s", title, content,category,food);
	}
```
	
# Spring MVC (스프링 웹 MVC) 강의 44 - POST 입력 #6(물리경로 얻기와 파일 저장하기)
* 
