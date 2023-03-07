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
*
