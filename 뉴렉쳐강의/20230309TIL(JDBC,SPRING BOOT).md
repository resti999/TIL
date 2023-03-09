# Spring JDBC 강의 01 - Service와 Dao 레이어의 이해
![image](https://user-images.githubusercontent.com/40667871/224024253-828d7b66-8cc5-4119-ab4f-4b4585f2cfb4.png)
![image](https://user-images.githubusercontent.com/40667871/224024584-9b820ce4-b211-43db-8ee1-ae00ed58cfdf.png)
![image](https://user-images.githubusercontent.com/40667871/224024730-bb3441d4-6f39-4ed6-8244-31b4546d2899.png)
* dao를 쓴다는건 sql를 service에서 하지 않는다는것
![image](https://user-images.githubusercontent.com/40667871/224025042-4d2190cd-5b74-46d0-ad47-dddd28c5f496.png)
* dao를 쓸 때의 장점 service를 구현하는 사람은 sql을 몰라도 됨->분업을 위해\
* Controller입장에서 NoticeService는 하나의 총판이다?
![image](https://user-images.githubusercontent.com/40667871/224026094-bf2c5f30-0cf2-471b-abe8-665e5e73a9be.png)
* 전체 골격
![image](https://user-images.githubusercontent.com/40667871/224026144-4656518c-784f-47ad-b3e9-5ca0e9a4c0e6.png)
* dao는 databse table과 1:1관계를 맺는다
* dao를 통해 table을 조작할 때 단조롭고 반복적이고 코드량만 많다-> 해결해주는 lib jdbc, mybatis, jpa/hb등이 있다.

# Spring JDBC 강의 02 - JdbcTemplate을 이용해 getList 메소드 구현하기
* 그냥 jdbc와 spring jdbc코드량 차이
* spring에서 model을 통해 view로 값 전달하는 방법.
![image](https://user-images.githubusercontent.com/40667871/224027885-2613131a-5c95-470a-bd50-80218242e486.png)
* JDBC Template 사용법
![image](https://user-images.githubusercontent.com/40667871/224029507-a01a36d3-0be6-467a-9ee2-e7149dacc27c.png)

# Sping Boot 2.x Quick Start 강의 03 - Spring Tools 4 for Eclipse 다운로드
* Spring 다른 강의에서 다운 완료

# Sping Boot 2.x Quick Start 강의 04 - Spring Boot Starter 프로젝트 만들기
* 

