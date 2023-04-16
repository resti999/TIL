# Sping Boot 2.x Quick Start 강의 41 - Service 계층 구현하기
* updatePubAll Dao 만들 때 기존의 서비스 함수의 형태에서 변경된 내용 반영
* dao를 변경하면서 변경 필요한 service함수 내용들 변경
* com.newlecture.web.dao.NoticeDao : getCount에 boolean bub 매개변수 추가
```
package com.newlecture.web.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Select;

import com.newlecture.web.entity.Notice;


@Mapper //이 어노테이션을 읽어서 IoC컨테이너에 객체 담아줌
public interface NoticeDao {
	

	List<Notice> getList(int startNum, int endNum, String field, String query, boolean pub);
	int getCount(String field, String query, boolean pub);
	Notice get(int id);
		
	Notice getNext(int id);
	Notice getPrev(int id);
	
	
	int update(Notice notice);
	int insert(Notice notice);
	int delete(int id);
	
//	int updatePubAll(int[] pubIds, int[] closeIds);
	int updatePubAll(int[] ids, boolean pub);
	int deleteAll(int[] ids);
}

```
* com.newlecture.web.dao.mybatis.NoticeDao : getCount 메서드에 boolean pub 추가
```
package com.newlecture.web.dao.mybatis;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.newlecture.web.dao.NoticeDao;
import com.newlecture.web.entity.Notice;

@Repository
public class MybatisNoticeDao implements NoticeDao {
	
	
	
	private NoticeDao mapper;
	
	@Autowired
	public MybatisNoticeDao(SqlSession sqlSession) {
		mapper = sqlSession.getMapper(NoticeDao.class);
		
	}
	
	@Override
	public List<Notice> getList(int startNum, int endNum, String field, String query, boolean pub) {
		// TODO Auto-generated method stub
		return mapper.getList(startNum, endNum, field, query, pub);
	}

	@Override
	public int getCount(String field, String query, boolean pub) {
		// TODO Auto-generated method stub
		return mapper.getCount(field, query, pub);
	}

	@Override
	public Notice get(int id) {
		// TODO Auto-generated method stub
		return mapper.get(id);
	}

	@Override
	public Notice getNext(int id) {
		// TODO Auto-generated method stub
		return mapper.getNext(id);
	}

	@Override
	public Notice getPrev(int id) {
		// TODO Auto-generated method stub
		return mapper.getPrev(id);
	}

	@Override
	public int update(Notice notice) {
		// TODO Auto-generated method stub
		return mapper.update(notice);
	}

	@Override
	public int insert(Notice notice) {
		// TODO Auto-generated method stub
		return mapper.insert(notice);
	}

	@Override
	public int delete(int id) {
		// TODO Auto-generated method stub
		return mapper.delete(id);
	}

	@Override
	public int updatePubAll(int[] ids, boolean pub) {
		// TODO Auto-generated method stub
		return mapper.updatePubAll(ids, pub);
	}

	@Override
	public int deleteAll(int[] ids) {
		// TODO Auto-generated method stub
		return mapper.deleteAll(ids);
	}

}

```
* com.newlecture.web.dao.mybatis.mapper.NoticeDaoMapper.xml : getCount메서드 resultType 반환값 int로 지정 후 getList에서 사용한 조건문 추가
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
		    <trim prefix="WHERE" prefixOverrides="AND | OR ">
		    <if test="query != null and query != ''">
		    		${field} LIKE '%${query}%'
		    </if>
		    		AND PUB = #{pub}
		    </trim>
		    ORDER BY REGDATE DESC, ID DESC
		    ) TEMP
		) TEMP
		WHERE NUM BETWEEN #{startNum} AND #{endNum}
  </select>
  <select id="getCount" resultType="int">
  		SELECT COUNT(id) count FROM NOTICE
  		<where>
		    <if test="query != null and query != ''">
		    		${field} LIKE '%${query}%'
		    </if>
		    		AND PUB = #{pub}
		</where>
  </select>
  <select id="get" resultType="com.newlecture.web.entity.Notice">
  		SELECT * FROM NOTICE
  		WHERE ID = #{id}
  </select>
  
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
  
  <delete id="deleteAll">
  		DELETE FROM NOTICE
  		WHERE ID IN 
		 <foreach item="id" index="index" collection="ids"
	        open="ID in (" separator="," close=")" >
	          #{id}
   		 </foreach>
  </delete>
  
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
  
  <!-- <update id="updatePubAll">
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
  </update> -->
  
  
</mapper>
```
* com.newlecture.web.service.NoticeService : getCount메서드에 boolean pub 매개변수 추가
```
package com.newlecture.web.service;

import java.util.List;

import com.newlecture.web.entity.Notice;

public interface NoticeService {

	// 페이지를 요청할 때
	List<Notice> getList( boolean pub);

	// 검색을 요청할 때
	List<Notice> getList(String field, String query, boolean pub);

	// 페이지를 요청할 때
	List<Notice> getList(int page, String field, String query ,boolean pub);

	int getCount();

	int getCount(String field, String query, boolean pub);

	// 자세한 페이지 요청할 때
	Notice getNext(int id);

	Notice getPrev(int id);

	Notice get(int id);

	// 일괄공개를 요청할 때
	int updatePubAll(int[] pubIds, int[] closeIds);

	// 일괄삭제를 요청할 때
	int deleteAll(int[] ids);

	// 수정 페이지를 요청할 때
	int update(Notice notice);

	int delete(int id);

	int insert(Notice notice);

}

```
* com.newlecture.web.service.NoticeServiceImp : getCount 메서드에 boolean pub 매개변수 추가, updatePubAll 메서드 내용 dao pubId와 closeId 두 번 호출하는 걸로 수정
```
package com.newlecture.web.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.newlecture.web.dao.NoticeDao;
import com.newlecture.web.entity.Notice;


@Service
public class NoticeServiceImp implements NoticeService {
	
	@Autowired
	private NoticeDao noticeDao;
	


	@Override
	public List<Notice> getList( boolean pub) {
		return getList(1, "title","", pub);
	}

	@Override
	public List<Notice> getList(String field, String query, boolean pub) {
		return getList(1, field, query, pub);
	}
	
	@Override
	public List<Notice> getList(int page, String field, String query, boolean pub) {
		
		int size=10;
		int startNum=1+(page-1)*size; //page 1->0, 2->10
		int endNum=page*size; //page 1->0, 2->10
		
		
		
		List<Notice> list = noticeDao.getList(startNum,endNum,field,query, pub);
		
		
		return list;
	}
	
	@Override
	public Notice get(int id) {
		
		Notice notice = noticeDao.get(id);
		
		return notice;
	}
	
	@Override
	public int getCount() {
		return getCount("title","",true);
	}

	@Override
	public int getCount(String field, String query, boolean pub) {
		return noticeDao.getCount(field,query,pub);
	}

	@Override
	public Notice getNext(int id) {
		return noticeDao.getNext(id);
	}

	@Override
	public Notice getPrev(int id) {
		return noticeDao.getPrev(id);
	}

	@Override
	public int updatePubAll(int[] pubIds, int[] closeIds) {
		int result = 0;
		result += noticeDao.updatePubAll(pubIds, true);
		result += noticeDao.updatePubAll(closeIds, false);
		return result;
	}

	@Override
	public int deleteAll(int[] ids) {
		// TODO Auto-generated method stub
		return noticeDao.deleteAll(ids);
	}

	@Override
	public int update(Notice notice) {
		return noticeDao.update(notice);
	}

	@Override
	public int delete(int id) {
		// TODO Auto-generated method stub
		return noticeDao.delete(id);
	}

	@Override
	public int insert(Notice notice) {
		// TODO Auto-generated method stub
		return noticeDao.insert(notice);
	}
	
}

```
