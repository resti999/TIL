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
```
 <delete id="deleteAll">
  		DELETE FROM NOTICE
  		WHERE ID IN 
		 <foreach item="id" index="index" collection="ids"
	        open="ID in (" separator="," close=")" >
	          #{id}
   		 </foreach>
  </delete>
```
* 키포인트 : 배열을 매개변수로 받는경우?  WHERE ID IN () 사용,  배열을 어떻게 , 로 나눈 문자열로 만들 수 있을까?-> 해결: Mybatis dynamic sql의 foreach문
* 배열이 foreach문의 collection에 들어감 item 의 변수로 하나씩 받음 ,반복 시작시 ( 열리고 끝날 때 ) 포함해줌, 그리고 구분자로 무엇을 넣을지 seperator로 설정 가능

# Sping Boot 2.x Quick Start 강의 35 - Dynamic SQL Mapper 구현하기 #5 (updatePubAll)
* updatePubAll 구현
* int updatePubAll(int[] pubIds, int[] closeIds);을 sql로 처리하면 배열 두개를 처리해야돼서 번거로움 일단 구현해보고 	int updatePubAll(int[] ids, boolean pub);, updatePubAll(int[] ids, boolean pub); 로 변형해서 한번 더 구현(다음시간) -> 하지만 두번실행으로 구현해야 해서 트랜잭션이 깨지는 문제 발생->트랜잭션처리 설명 예정
* NoticeDaoMapper.xml 구현내용 : updatePubALl , 매개변수에 배열이 두개가 와서 foreach문을 각 배열당 하나씩 써야 하는 번거로움 존재
```
<update id="updatePubAll">
  		UPDATE NOTICE
  		SET
  			PUB = CASE ID
  						<foreach item="id" collection="pubIds">
  							WHEN #{id} THEN 1
  						</foreach>
  						<foreach item="id" collection="closeIds">
  							WHEN #{id} THEN 0
  						</foreach>
  						END
  		WHERE ID IN (
  			<foreach item="id" collection="pubIds" separator=",">
				#{id}
			</foreach>
  			,
  			<foreach item="id" collection="closeIds" separator=",">
				#{id}
			</foreach>
  		)
  </update>
```

# Sping Boot 2.x Quick Start 강의 36 - Dynamic SQL Mapper 구현하기 #6 추가된 (updatePubAll)
* updatePubAll pubIds, closeIds로 나눠서 구현
* Mybatis는 overload지원 X -> 함수의 이름이 달라야함
* NoticeDaoMapper.xml 구현내용 : int updatePubAll(int[] ids, boolean pub);함수 구현
```
<update id="updatePubAll">
  		UPDATE NOTICE
  		SET
  			PUB = #{pub}
  		WHERE ID IN 
		<foreach item="id" index="index" collection="ids"
	        open="ID in (" separator="," close=")" >
	          #{id}
   		 </foreach>
  </update>
```

# Sping Boot 2.x Quick Start 강의 37 - Dynamic SQL (if, where, trim 태그)
* Dynamic SQL 로 처음 구현한 List<Notice> getList(int startNum, int endNum, String field, String query) mybatis의 query문 수정
* getList 함수에서 관리자는 모든 목록을 봐야하지만 고객은 pub된 것 만 봐야하는 문제 ->**dao함수에 boolean pub매개변수 추가**
* NoticeDaoMapper.xml 구현내용 - trim버전
```
<select id="getList" resultType="com.newlecture.web.entity.Notice">
		  	SELECT * FROM
		(
		    SELECT ROWNUM NUM, TEMP.*
		    FROM
		    (
		    SELECT *
		    FROM NOTICE
		    <trim prefix="WHERE" prefixOverrides="AND \ OR ">
		    <if test="query != null or query != ''">
		    		${field} LIKE '%${query}%'
		    </if>
		    		AND PUB = #{pub}
		    </trim>
		    ORDER BY REGDATE DESC, ID DESC
		    ) TEMP
		) TEMP
		WHERE NUM BETWEEN #{startNum} AND #{endNum}
  </select>
```
* query 매개변수가 ''이나 null아닐때만 실행
* where 다음 조건문이 빠지면 구문이 이상해지는 오류 발생 and 부터 시작 ->Dynamic SQL에 where절이 따로 있음 Dynamic SQL에서 자체적으로 조건문 진행 -> AND 나 OR를 자동으로 지워줌
```
<where>
		    <if test="query != null or query != ''">
		    		${field} LIKE '%${query}%'
		    </if>
		    		AND PUB = #{pub}
</where> 
```
* 혹시 where절이 동작하지 않으면 trim절 사용 -> trim 절 안쪽에 구문이 붙으면 prefix가 붙음, 만약 구문 시작 글자가 and, or이면 prefix로 대체
* 다음시간엔 xml 에 컬럼맵핑 어떻게 하는지 안내
