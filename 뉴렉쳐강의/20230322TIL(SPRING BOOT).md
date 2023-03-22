# Sping Boot 2.x Quick Start 강의 33 - Mapper 구현하기 #3 (insert/update/delete)
* NoticeDaoMapper.xml 구현내용: insert/update/delete 구현
```
<update id="update" parameterType="com.newlecture.web.entity.Notice">
  		UPDATE
  		SET
  			TITLE = #{title},
  			CONTENT = #{content},
  			HIT = #{hit},
  			PUB = #{pub}
		WHERE ID = #{id}
  </update>
  
  <insert id="insert" parameterType="com.newlecture.web.entity.Notice">
  		INSERT INTO NOTICE(title,content,memberId)
  		VALUES(#{title},#{content},#{memberId})
  </insert>
  		
  <delete id="delete">
  		DELETE FROM NOTICE
  		WHERE ID = #{id}
  </delete>
  
```

# Sping Boot 2.x Quick Start 강의 34 - Dynamic SQL Mapper 구현하기 #4 (deleteAll)
* NoticeDaoMapper.xml 구현내용 : delete All 함수 구현
* 키포인트 : 배열을 매개변수로 받는경우?  WHERE ID IN () 사용,  배열을 어떻게 , 로 나눈 문자열로 만들 수 있을까?-> 해결: Mybatis dynamic sql의 foreach문
