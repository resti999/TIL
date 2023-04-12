# Sping Boot 2.x Quick Start 강의 38 - Mapper 객체 단위 Test with jUnit5와 XML에 resultMap 사용하기- 08:08이어서
* test class 는 src/test/java 안에 있음
* @AutoConfigureTestDatabase(replace = Replace.NONE) : test위한 데이터베이스를 따로 마련하지 않았으므로 기존 데이터베이스 연결정보를 사용한다는 의미
* DB의 컬럼명과 entity의 필드 이름이 다를 경우 NoticeDaoMapper.xml에서 설정필요 resultMap태그 설정 후에 select 태그의 resultType속성이 아닌 resultMap속성에 resultMap id 속성의 값을 넣으면 된다. -entity담긴 내용이 null로 담겼을 때 컬럼명과 다른지 의심 필요
```
mapper tag안에
<resultMap type="com.newlecture.web.entity.NoticeView" id="noticeMap">
  <result property="memberName" column="member_name"/>
</resultMap>
<select id="getviewList" resultMap="noticeViewMap"> ...
```

# Sping Boot 2.x Quick Start 강의 39 - Dao 구현체 직접 구현하기 SqlSession
