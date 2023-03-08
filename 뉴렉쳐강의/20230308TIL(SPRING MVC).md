# Spring MVC (스프링 웹 MVC) 강의 45 - POST 입력 #7(다중파일 업로드)
* 첨부파일 input이 2개 일 경우? name이 같으면 배열로 전달됨.
* Multipart배열로 받고 files로 받는다. 그 후 전 예제에서 for문만 추가하면 된다.
* NoticeController 변경사항
```
package com.newlecture.web.controller.customer.admin.board;

import java.io.File;
import java.io.IOException;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller("adminNoticeController")
@RequestMapping("/admin/board/notice/")
public class NoticeController { // <bean name="noticeController" classes=""
	
	@RequestMapping("list")
	public String list() {
		return "";
	}
	
	@RequestMapping("reg")
	@ResponseBody
	public String reg(String title, String content,MultipartFile[] files, String category, String[] foods, String food, HttpServletRequest request) throws IllegalStateException, IOException {
		
		for(MultipartFile file : files) {
			long size = file.getSize();
			String fileName = file.getOriginalFilename();
			System.out.printf("fileName:%s, fileSize:%d\n", fileName, size);
			
			//파일 저장을 위해 경로지정
			ServletContext ctx=request.getServletContext();
			String webPath = "/static/upload";
			String realPath = ctx.getRealPath(webPath);
			System.out.printf("realPath: %s\n", realPath);
			//파일 업로드할 폴더 있는지 확인
			File savePath = new File(realPath);
			if(!savePath.exists()) // 경로 실제 존재 확인 -> 경로가 없으면
				savePath.mkdirs(); // mkdir와 다르게 중간에 빠진 경로 있으면 다 만들어줌
			//os별 path 구분자로 경로 뒤에 파일 이름 넣기
			realPath+=File.separator+fileName;
			File saveFile = new File(realPath);
			//multipart 객체를 통해 파일 저장
			file.transferTo(saveFile);
		}
		
		
		for(String food1 : foods)
			System.out.println(food1);
		
		return String.format("title:%s<br> content:%s<br>category:%s<br>food:%s", title, content,category,food);
	}
	
	@RequestMapping("edit")
	public String edit() {
		return "";
	}
	
	@RequestMapping("del")
	public String del() {
		return "";
	}
}

```

#
