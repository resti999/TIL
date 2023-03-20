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
