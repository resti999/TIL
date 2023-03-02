# Spring MVC (스프링 웹 MVC) 강의 23 - 연결정보 분리하기
* JDBCNoticeService의 db커넥정보를 코드로 입력했는데, 나중에 배포 후 바이너리 코드일 때 db비밀번호가 바뀌면 비밀번호를 변경하기 곤란해진다.
* 커넥션 정보 spring xml파일로 변경
* JDBCNoticeService : 커넥션  정보 지운 후 datasource인터페이스 생성 후   커넥션 때 사용
```
package com.newlecture.web.service.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.sql.DataSource;

import com.newlecture.web.entity.Notice;
import com.newlecture.web.service.NoticeService;

public class JDBCNoticeService implements NoticeService {
//	private String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
//	private String uid = "NEWLEC";
//	private String pwd = "24rkwk";
//	private String driver = "oracle.jdbc.driver.OracleDriver";
	
	private DataSource dataSource;
	
	public void setDataSource(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public List<Notice> getList(int page, String field, String query) throws ClassNotFoundException, SQLException{
		
		int start = 1 + (page-1)*10;     // 1, 11, 21, 31, ..
		int end = 10*page; // 10, 20, 30, 40...
		
		String sql = "SELECT * FROM NOTICE_VIEW WHERE "+field+" LIKE ? AND NUM BETWEEN ? AND ?";	
//		String sql = "select * from "
//				+ "("
//				+ "    select rownum num ,id, title, writer_id, regdate, hit, files, pub,cmt_count "
//				+ "    from "
//				+ "    ("
//				+ "        select * from notice_view order by regdate desc, id desc"
//				+ "    ) main"
//				+ ") main "
//				+ "where "+field+" like ? and num between ? and ?";
		
		
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, "%"+query+"%");
		st.setInt(2, start);
		st.setInt(3, end);
		ResultSet rs = st.executeQuery();
		
		List<Notice> list = new ArrayList<Notice>();
		
		while(rs.next()){
		    int id = rs.getInt("ID");
		    String title = rs.getString("TITLE");
		    String writerId = rs.getString("WRITER_ID");
		    Date regDate = rs.getDate("REGDATE");
		    String content = rs.getString("CONTENT");
		    int hit = rs.getInt("hit");
		    String files = rs.getString("FILES");
		    
		    Notice notice = new Notice(
		    					id,
		    					title,
		    					writerId,
		    					regDate,
		    					content,
		    					hit,
		    					files
		    				);

		    list.add(notice);
		    
		}

		
		rs.close();
		st.close();
		con.close();
		
		return list;
	}
	
	// Scalar 
	public int getCount() throws ClassNotFoundException, SQLException {
		int count = 0;
		
		String sql = "SELECT COUNT(ID) COUNT FROM NOTICE";	
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		Statement st = con.createStatement();
		
		ResultSet rs = st.executeQuery(sql);
		
		if(rs.next())
			count = rs.getInt("COUNT");		
		
		rs.close();
		st.close();
		con.close();
		
		return count;
	}

	public int insert(Notice notice) throws SQLException, ClassNotFoundException {
		String title = notice.getTitle();
		String writerId = notice.getWriterId();
		String content = notice.getContent();
		String files = notice.getFiles();
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "INSERT INTO notice (    " + 
				"    title," + 
				"    writer_id," + 
				"    content," + 
				"    files" + 
				") VALUES (?,?,?,?)";	
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, title);
		st.setString(2, writerId);
		st.setString(3, content);
		st.setString(4, files);
		
		int result = st.executeUpdate();
		
		
		st.close();
		con.close();
		
		return result;
	}
	
	public int update(Notice notice) throws SQLException, ClassNotFoundException {
		String title = notice.getTitle();
		String content = notice.getContent();
		String files = notice.getFiles();
		int id = notice.getId();
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "UPDATE NOTICE " + 
				"SET" + 
				"    TITLE=?," + 
				"    CONTENT=?," + 
				"    FILES=?" + 
				"WHERE ID=?";
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, title);
		st.setString(2, content);
		st.setString(3, files);
		st.setInt(4, id);
		
		int result = st.executeUpdate();
				
		st.close();
		con.close();
		
		return result;
	}
	
	public int delete(int id) throws ClassNotFoundException, SQLException {
	
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "DELETE NOTICE WHERE ID=?";
		
//		Class.forName(driver);
//		Connection con = DriverManager.getConnection(url,uid, pwd);
		Connection con = dataSource.getConnection();
		
		
		
		//Statement st = con.createStatement();
		//st.ex....(sql)
		PreparedStatement st = con.prepareStatement(sql);		
		st.setInt(1, id);
		
		int result = st.executeUpdate();
				
		st.close();
		con.close();
		
		return result;
	}

	
}

```

* spring의 data처리해주는 lib 추가(spring-jdbc)
```
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.12.RELEASE</version>
</dependency>
```
* dispatcher-servlet.xml 에 JDBCNoticeService에 dataSource property추가 및  dataSource객체 생성 후  커넥션 정보 property에 추가
```
<bean id="noticeService" class="com.newlecture.web.service.jdbc.JDBCNoticeService">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
		<property name="url" value="jdbc:oracle:thin:@localhost:1521/xepdb1"/>
		<property name="username" value="NEWLEC"/>
		<property name="password" value="24rkwk"/>
	</bean>
```
*
