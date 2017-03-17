# Mybatis doesn't support query multiple ResultSets via HANA JDBC driver

### Environment
> * Mybatis 3.4.2
> * HANA JDBC driver: ngdbc 1.120.14

### Procedure  
``` SQL
CREATE TYPE "I321761"."tt_person" AS TABLE (
   id INTEGER,
   name VARCHAR(100));
   
CREATE TYPE "I321761"."tt_company" AS TABLE (
   id INTEGER,
   address VARCHAR(100));
   
CREATE PROCEDURE "I321761"."TEST"(in id integer, out person "I321761"."tt_person", out company "I321761"."tt_company" )
    LANGUAGE SQLSCRIPT
    SQL SECURITY INVOKER 
    as
begin
    person = select id*2 as id, 'tom' name from dummy;
    company = select id/2 as id, 'nanjing' address from dummy;
end;  
```

### Mapper  
``` xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
<mapper namespace="com.mybatis3.mappers.TestMapper">

    <resultMap id="personResultMap" type="com.sap.icn.plc.model.Person">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
    </resultMap>

    <resultMap id="companyResultMap" type="java.util.HashMap">
        <result property="id" column="id"/>
        <result property="address" column="address"/>
    </resultMap>

    <select id="getTest" statementType="CALLABLE" parameterType="java.util.Map" resultMap="personResultMap, companyResultMap">
        { call "I321761"."TEST"(#{id}, ?, ?) }
    </select>
</mapper>  
```  

### Service  
``` java
@Service
public class TestService {
    @Autowired
    SqlSessionFactory sqlSessionFactory;

    public Test getTest() {
        SqlSession sqlSession = sqlSessionFactory.openSession();

        Map<String, Object> params = new HashMap();
        params.put("id", 125);

        List<List<?>> ps = sqlSession.selectList("com.mybatis3.mappers.TestMapper.getTest", params);
        sqlSession.close();
        return null;
    }
}  
```

### Cause analyse
* MyBatis uses `org.apache.ibatis.executor.resultset.DefaultResultSetHandler` to process statement execution result.
  In this handler, it will check if database supports multiple resultsets via JDBC API: ```connection.getMetaData().supportsMultipleResultSets() ```,
  if it return false, then Mybatis will not ```getMoreResultSet```.
  This is the key point, for HANA JDBC driver, this method always returns false.
  And I did another test, use MySQL JDBC driver to connect a MYSQL database, this method returns true.
  So it is a bug of HANA JDBC driver.
  ``` java
  // DefaultResultHandler
  private ResultSetWrapper getNextResultSet(Statement stmt) throws SQLException {
    // Making this method tolerant of bad JDBC drivers
    try {
      if (stmt.getConnection().getMetaData().supportsMultipleResultSets()) {
        // Crazy Standard JDBC way of determining if there are more results
        if (!((!stmt.getMoreResults()) && (stmt.getUpdateCount() == -1))) {
          ResultSet rs = stmt.getResultSet();
          return rs != null ? new ResultSetWrapper(rs, configuration) : null;
        }
      }
    } catch (Exception e) {
      // Intentionally ignored.
    }
    return null;
  }
  ```

