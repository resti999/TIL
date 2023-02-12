# 자바 인강 서블릿/JSP 강의/강좌 107 - 오버로드 서비스 메소드 구현하기
* 단순히 for문으로 해결되는 것을 library사용하는 것은 좋지 않은 습관이다.
* pubNoticeAll함수의 오버로드 함수들 먼저 구현(데이터 형식변환하는 로직들)
```
public int pubNoticeAll(int[] oids, int[] cids) {
		
		//oids int형 배열을 String 컬렉션으로 변환
		List<String> oidsList = new ArrayList<>();
		for(int i=0; i<oids.length; i++)
			oidsList.add(String.valueOf(oids[i]));
		
		//cids int형 배열을 String 컬렉션으로 변환
		List<String> cidsList = new ArrayList<>();
		for(int i=0; i<cids.length; i++)
			oidsList.add(String.valueOf(cids[i]));
		
		return pubNoticeAll(oidsList, cidsList);
	}
	public int pubNoticeAll(List<String> oids, List<String> cids) {
		
		//매개변수로 받은 Collection 들을 String.join함수를 통해 ,를 구분자로 한 문자열로 만듬
    //String 배열을 csv형태로 만들기(배열형태론 안돼서 Collection으로 만들어야한다.)
		String oidsCSV = String.join(",",oids) ;
		String cidsCSV = String.join(",",cids);
		
		
		return pubNoticeAll(oidsCSV, cidsCSV);
	}
	// 20,30,42,56  아직 미구현
	public int pubNoticeAll(String oidsCSV, String cidsCSV) {
		
		String sql1 = "UPDATE NOTICE SET PUB=1 WHERE ID IN (?);"
		return 0;
	}
	
```

# 자바 인강 서블릿/JSP 강의/강좌 108 - pubNoticeAll 구현하기
* NoticeService class의 pubNoticeAll 함수구현
```
public int pubNoticeAll(String oidsCSV, String cidsCSV) {
		
		int result =0;
		
//		String sqlOpen = "UPDATE NOTICE SET PUB=1 WHERE ID IN ("+oidsCSV+")"; 아래와 동일
		//pritf 와 같은 느낌의 함수 String.format
		String sqlOpen = String.format("UPDATE NOTICE SET PUB=1 WHERE ID IN (%s)", oidsCSV);
		String sqlClose = String.format("UPDATE NOTICE SET PUB=0 WHERE ID IN (%s)", cidsCSV);
		
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url,"newlec","24rkwk");
			Statement stOpen = con.createStatement();
			result += stOpen.executeUpdate(sqlOpen);
			
			Statement stClose = con.createStatement();
			result += stClose.executeUpdate(sqlClose);
			
			
			stOpen.close();
			stClose.close();
			con.close();

		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return result;
	}
```

# 스프링 프레임워크 강의 1강 - Spring 소개와 학습 안내
* 스프링 프레임워크의 핵심 기능 2가지 
   * Dependency Injection
   * Transaction Management : JDBC자체의 Transaction 관리는 보통 connect 객체를 통해 이뤄지는데 DAO, Service를 구분할 경우 jdbc자체의 기능인 connect 객체를 이용해 Transaction처리를 하는 것은 어렵다.
* JAVA Edition 중 JAVA EE는 Dependcy Injection, Transaction Management등을 지원했지만 스프링보다 복잡->스프링이 EE영역을 잠식 -> 원래 java se에 java ee를 얹어서 사용했다면 java ee 기능을 spring으로 대체해서 사용
* 배울것과 관련된 대표적인 용어
![image](https://user-images.githubusercontent.com/40667871/218296788-1586e3d1-b54d-4f25-b2bc-e41e98f14d78.png)
