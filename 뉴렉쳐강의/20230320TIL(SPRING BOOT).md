# Sping Boot 2.x Quick Start 강의 26 - Mapper 에 Parameter 사용하기
* parameter 가 있을 때 Mapper활용법
* NoticeController  - service로 전달할 변수들 선언 및 초기화
```
package com.newlecture.web.controller.customer;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

@Controller
@RequestMapping("/customer/notice/")
public class NoticeController {
	
	@Autowired
	private NoticeService service;
	
	
	@RequestMapping("list")
	public String list(Model model) {
		
		int page = 3;
		String field = "title";
		String query ="";
		
		
		List<Notice> list = service.getList(page,field,query);
		
		model.addAttribute("list", list);
		
//		return "/customer/notice/list"; ResourceViewResolver
		return "customer.notice.list";  // TilesViewResolver
	}
	
	@RequestMapping("detail")
	public String detail() {
		return "customer.notice.detail";
	}
}

```
* NoticeServie :controller에서 전달받는 만큼 parameter 변경. interface, implements 둘다 . implements만 예시, 또한 dao의 쿼리에 맞는 페이징 데이터 준비
```
package com.newlecture.web.controller.customer;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

@Controller
@RequestMapping("/customer/notice/")
public class NoticeController {
	
	@Autowired
	private NoticeService service;
	
	
	@RequestMapping("list")
	public String list(Model model) {
		
		int page = 3;
		String field = "title";
		String query ="";
		
		
		List<Notice> list = service.getList(page,field,query);
		
		model.addAttribute("list", list);
		
//		return "/customer/notice/list"; ResourceViewResolver
		return "customer.notice.list";  // TilesViewResolver
	}
	
	@RequestMapping("detail")
	public String detail() {
		return "customer.notice.detail";
	}
}

```
* DAO : DAO는 Interface그자체 DAO에선 쿼리 부분을 준비, query문 쓸 때 값을 넣는 부분은 값은 #{}, ''이 없어야 하는 명령부는 ${} 에 변수 입력 *querystring중요하게 볼 것*
```
package com.newlecture.web.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Select;

import com.newlecture.web.entity.Notice;


@Mapper //이 어노테이션을 읽어서 IoC컨테이너에 객체 담아줌
public interface NoticeDao {
	
	
	@Result(property = "writerId", column="writer_id")
	@Select("SELECT * FROM "
			+ "("
			+ "    SELECT ROWNUM NUM, TEMP.* "
			+ "    FROM "
			+ "    ( "
			+ "    SELECT * "
			+ "    FROM NOTICE "
			+ "    WHERE ${field} LIKE '%${query}%' "
			+ "    ORDER BY REGDATE DESC, ID DESC "
			+ "    ) TEMP "
			+ ") TEMP "
			+ "WHERE NUM BETWEEN #{startNum} AND #{endNum}")
	List<Notice> getList(int startNum, int endNum, String field, String query);
	
	
	Notice get(int id);

}

```

# Sping Boot 2.x Quick Start 강의 27 - XML을 이용한 매핑
* annotation mapping은 규모가 클 때 문제
   * qeury문제가 복잡 할 수록 
   * interface인데   특정 lib인 마이바티스와 너무 긴밀하게 연결됨 ->문제, 나중에 교체 힘듦
   * 쿼리문 비중이 커서  interface함수가 잘 안보임
* mapping을 xml로 하는게 바람직
* com.newlecture.web.dao.mybatis.mapper.NoticeDaoMapper.xml 생성
* namespace에 interface 풀네임 쓰기
* select,insert에 맞게 태그 생성 후 id 속성에 해당 메서드 이름 적기
* 태그 속성 resultType에 query의 결과를 어떤 entity에 담을지 풀 class명 써주기, list인지 단일값인지 구분은 자동으로 해준다.
* select가 아닌 update,insert등 반대로 entity 데이터로 db정보 바꿀 땐 parameterType 속성 사용
* application.properties 에 mapper xml파일 위치 알려주기
* com.newlecture.web.dao.NoticeDao
```
package com.newlecture.web.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Select;

import com.newlecture.web.entity.Notice;


@Mapper //이 어노테이션을 읽어서 IoC컨테이너에 객체 담아줌
public interface NoticeDao {
	
	

	List<Notice> getList(int startNum, int endNum, String field, String query);
	
	
	Notice get(int id);
	
	int update(Notice notice);
	int insert(Notice notice);
	int delete(Notice notice);

}

```
* com,newlecture.web.dao.mybatis.mapper.*.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.newlecture.web.dao.NoticeDao">
  <select id="getList" resultType="com.newlecture.web.entity.Notice">
		  	SELECT * FROM
		(
		    SELECT ROWNUM NUM, TEMP.*
		    FROM
		    (
		    SELECT *
		    FROM NOTICE
		    WHERE TITLE LIKE '%%'
		    ORDER BY REGDATE DESC, ID DESC
		    ) TEMP
		) TEMP
		WHERE NUM BETWEEN 11 AND 20
  </select>
</mapper>
```
* application.properties
```
spring.mvc.view.prefix=/WEB-INF/view
spring.mvc.view.suffix=.jsp

#spring.datasource.url=jdbc:oracle:thin:@localhost:1521/xepdb1
#spring.datasource.username=newlec
#spring.datasource.password=24rkwk
#spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver

mybatis.mapper-locations=classpath:com/newlecture/web/dao/mybatis/mapper/*.xml
```

# Sping Boot 2.x Quick Start 강의 28 - XML Proposals 오류 문제
* mybatis xml 의 dtd 파일은 사용할 태그들을 정의하고 있는 문서 -> 편집기의 proposal이 동작을 안하는 문제 발생
* 해결법
   * help
   * xml 검색
   * Eclipse XML Editors 설치

# Sping Boot 2.x Quick Start 강의 29 - NoticeService 인터페이스 정의하기
* 
