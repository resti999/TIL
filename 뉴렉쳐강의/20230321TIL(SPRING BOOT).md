# Sping Boot 2.x Quick Start 강의 31 - Mapper 구현하기 #1 (getCount/getView)
* NoticeDaoMapper.xml : getCount, get DAO 구현
```
<select id="getCount">
  		SELECT COUNT(id) count FROM NOTICE
  		WHERE ${field} like '%${query}%'
  </select>
  <select id="get" resultType="com.newlecture.web.entity.Notice">
  		SELECT * FROM NOTICE
  		WHERE ID = #{id}
  </select>
```

# Sping Boot 2.x Quick Start 강의 32 - Mapper 구현하기 #2 (getNext/getPrev)
![image](https://user-images.githubusercontent.com/40667871/226625526-3d94e965-167b-4b47-8ae4-fc323ea32723.png)
* 이런 상황에서 id 3의 위의 row를 구하려면?
```
SELECT ID FROM NOTICE
WHERE REGDATE >= (SELECT REGDATE FROM NOTICE WHERE ID=3)
AND ID>(SELECT ID FROM NOTICE WHERE ID=3)
AND ROWNUM=1;
```
* HTML XML등에선 꺾음쇠는 태그를 나타내는 특수한 기호기 때문에 크기비교를 나타내는 부호 >< 를 대신하는 ENTITY가 있음
* mybatis에서 ">=" 부등호를 사용하는 방법은 아래와 같다.
* ![image](https://user-images.githubusercontent.com/40667871/226632265-43b422f0-294a-45ce-9584-b047bccea27d.png)
* NoticeDaoMapper.xml : getNext, getPrev query 구현
```
<select id="getNext" resultType="com.newlecture.web.entity.Notice">
  		SELECT ID FROM NOTICE
		WHERE REGDATE &gt;= (SELECT REGDATE FROM NOTICE WHERE ID=#{id})
		AND ID&gt;#{id}
		AND ROWNUM=1
  </select>
  
  <select id="getPrev" resultType="com.newlecture.web.entity.Notice">
  		SELECT *
		FROM
		(
		SELECT ID FROM NOTICE
		WHERE REGDATE &lt;= (SELECT REGDATE FROM NOTICE WHERE ID=#{id})
		AND ID&lt;#{id}
		ORDER BY ID DESC
		) TEMP
		WHERE ROWNUM =1
  </select>
```
