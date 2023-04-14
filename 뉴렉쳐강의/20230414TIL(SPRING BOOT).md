# Sping Boot 2.x Quick Start 강의 40 - 나머지 Dao 구현체 직접 구현하기 Constructor Injection
* 구현체 클래스의 각 메서드에 일일히 NoticeDao noticeDa = sqlSession.getMapper(NoticeDao.class);를 입력하면 번거롭기 때문에 멤버변수로  NotiaceDao mapper 선언 이름을 mapper로 하는 이유는 헷갈리지 않기 위해서, 그리고 생성자에 해당 멤버변수에 sqlSession.get ~을 통해 객체를 넣어줌.
* setter 에 autowired 붙이면 setter injection, construct에 붙이면 construct injection
* 변경된 com.newlecture.web.dao.mybatis.MybatisNoticeDao
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
	public int getCount(String field, String query) {
		// TODO Auto-generated method stub
		return mapper.getCount(field, query);
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

# Sping Boot 2.x Quick Start 강의 41 - Service 계층 구현하기
*
