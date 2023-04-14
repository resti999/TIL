# Sping Boot 2.x Quick Start 강의 38 - Mapper 객체 단위 Test with jUnit5와 XML에 resultMap 사용하기- 08:08이어서
* test class 는 src/test/java 안에 있음
* @AutoConfigureTestDatabase(replace = Replace.NONE) : test위한 데이터베이스를 따로 마련하지 않았으므로 기존 데이터베이스 연결정보를 사용한다는 의미
* DB의 컬럼명과 entity의 필드 이름이 다를 경우 NoticeDaoMapper.xml에서 설정필요 resultMap태그 설정 후에 select 태그의 resultType속성이 아닌 resultMap속성에 resultMap id 속성의 값을 넣으면 된다. -entity담긴 내용이 null로 담겼을 때 컬럼명과 다른지 의심 필요
```
mapper tag안에
<resultMap type="com.newlecture.web.entity.NoticeView" id="noticeMap">
  <result property="memberName" column="member_name"/>
</resultMap>
<select id="getviewList" resultMap="noticeMap"> ...
```

# Sping Boot 2.x Quick Start 강의 39 - Dao 구현체 직접 구현하기 SqlSession
* AutoWired annotation은 IoC 컨테이너에 있는 객체를 가져다 사용할 수 잇게 해줌
* 기존에는 mapper라는 객체를 이용해서 구현함
![image](https://user-images.githubusercontent.com/40667871/231484743-0c88fd51-91df-46cd-9d1b-4d85f5a86ad6.png)
* SqlSeesionFactory . xml의 mapper정보를 읽어 Mapper container에 객체를 만드는 주체
* SqlSeesion : Mapper container에서 객체를 꺼내서  dao를 구현
* Spring boot로 오면서 dao객체를 생성하지 않아도 mapper를 ioc컨테이너에 담개해주는 것을 가능하게 @Mapper annotation이 생겼고 우리가 그것을 사용했던 것
* 그 전에는 dao를 구현하는 과정에서 mapper를 이용하는 절차로 진행 -> Mybatis는 dao를 구현하기 위해서 좋은 도구이지만 dao를 대신하는 녀석으 아님
* 기존 우리가 구현한 코드 : dao interface는 있지만 구현체는 없고 interface위에 @Mapper와 mapper.xml만으로 db연결이 됏었음
* 다양한 기술을 구현해서 dao를 구현할 수 있는데 dao interface가 @Mapper annotation 때문에 Mybatis에 강하게 결합되는 문제 발생
* @Mapper는 Mybatis의 SqlSessionFactory에 의해서 만들어지는 mapper객체들은 Mapper container라는 공간에 담김(여기까진 mybatis의 역할)  @Mapper는 그 공간의 객체들을 spring의 IoC container에 담아주는 역할
* NoticeDao interface의 구현체를 직접 만들것
   * com.newlecture.web.dao.mybatis.MybatisNoticeDao 클래스 생성 후 NoticeDao interface implements
   * 구현체에 SqlSession을 사용해서 Mapper객체 불러올 것임, SqlSession은 Mybatis lib에 의해서 IoC 컨테이너에 담겨있음
   * sqlSession.getMapper(NoticeDao.class) - NoticeDao의 구현체를 반환
   * MybatisNoticeDao  test 클래스 만들기(38강 참조)
* test class에서 spring이 깨어나서 spring context? 에 다양한 객체를 담아주기 위해선  @SpringBootTest  가 필요 , Mybastis에선 @MybatisTest였음
* Mybatis는 overload함수를 지원하지 않음
* com.newlecture.web.dao.mybatis.MybatisNoticeDao 구현내용
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
	
	@Autowired
	private SqlSession sqlSession;
	
	@Override
	public List<Notice> getList(int startNum, int endNum, String field, String query, boolean pub) {
		// TODO Auto-generated method stub
		NoticeDao noticeDao = sqlSession.getMapper(NoticeDao.class);
		return noticeDao.getList(startNum, endNum, field, query, pub);
	}

	@Override
	public int getCount(String field, String query) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public Notice get(int id) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public Notice getNext(int id) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public Notice getPrev(int id) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public int update(Notice notice) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int insert(Notice notice) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int delete(int id) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int updatePubAll(int[] ids, boolean pub) {
		// TODO Auto-generated method stub
		return 0;
	}

	@Override
	public int deleteAll(int[] ids) {
		// TODO Auto-generated method stub
		return 0;
	}

}

```
* com.newlecture.web.dao.mybatis.MybatisNoticeDaoTest 구현내용
```
package com.newlecture.web.dao.mybatis;

import java.util.List;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import com.newlecture.web.dao.NoticeDao;
import com.newlecture.web.entity.Notice;


@SpringBootTest
class MybatisNoticeDaoTest {
	
	@Autowired
	private NoticeDao noticeDao;
	
	
	@Test
	void test() {
		List<Notice> list = noticeDao.getList(0, 1, null, null, false);
		System.out.println(list.get(0));
	}

}

```
