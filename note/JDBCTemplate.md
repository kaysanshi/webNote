# JDBCTemplate

### JDBCTemplate


`org.springframework.jdbc.core.JdbcTemplate`类是JDBC核心包中的中心类。它简化了JDBC的使用，并有助于避免常见的错误。 它执行核心JDBC工作流，留下应用程序代码来提供SQL并提取结果。 该类执行SQL查询或更新，在`ResultSet`类上启动迭代并捕获JDBC异常，并将它们转换为`org.springframework.dao`包中定义的通用更详细的异常层次结构。使用这个类的代码只需要实现回调接口，给它们一个明确定义的协定。 Preparedstatementcreator 回调接口在给定 Connection 的情况下创建一个准备好的语句，提供 SQL 和任何必要的参数。 Resultsetextractor 接口从 ResultSet 中提取值。 请参阅 PreparedStatementSetter 和 RowMapper 了解两个流行的可选回调接口。

| `<T> T` | `execute(CallableStatementCreator csc, CallableStatementCallback<T> action)Execute a JDBC data access operation, implemented as callback action working on a JDBC CallableStatement. 执行 JDBC 数据访问操作，实现为在 JDBC CallableStatement 上工作的回调动作` |
| --- | --- |
| `<T> T` | `execute(ConnectionCallback<T> action)`Execute a JDBC data access operation, implemented as callback action working on a JDBC Connection. 执行 JDBC 数据访问操作，实现为在 JDBC 连接上工作的回调动作 |
| `<T> T` | `execute(PreparedStatementCreator psc, PreparedStatementCallback<T> action)`Execute a JDBC data access operation, implemented as callback action working on a JDBC PreparedStatement. 执行 JDBC 数据访问操作，实现为在 JDBC PreparedStatement 上工作的回调动作 |
| `<T> T` | `execute(StatementCallback<T> action)`Execute a JDBC data access operation, implemented as callback action working on a JDBC Statement. 执行 JDBC 数据访问操作，实现为对 JDBC 语句的回调动作 |
| `void` | `execute(String sql)`Issue a single SQL execute, typically a DDL statement. 发出一个 SQL 执行，通常是一个 DDL 语句 |
| `<T> T` | `execute(String callString, CallableStatementCallback<T> action)`Execute a JDBC data access operation, implemented as callback action working on a JDBC CallableStatement. 执行 JDBC 数据访问操作，实现为在 JDBC CallableStatement 上工作的回调动作 |
| `<T> T` | `execute(String sql, PreparedStatementCallback<T> action)`Execute a JDBC data access operation, implemented as callback action working on a JDBC PreparedStatement. 执行 JDBC 数据访问操作，实现为在 JDBC PreparedStatement 上工作的回调动作 |
|  |  |

#### 插入


```
String insertQuery = "insert into student (name, age) values (?, ?)";
jdbcTemplate.update( insertQuery, name, age)
```

#### 批量插入


```java
@Override
    public void batchCreateNoteCommentV2(final List<NoteCommentModel> noteComment) {
        jdbcTemplate.batchUpdate(addNoteCommentSql, new BatchPreparedStatementSetter() {
            @Override
            public void setValues(PreparedStatement ps, int i) throws SQLException {
                ps.setString(1, UUID.randomUUID().toString());
                ps.setString(2, noteComment.get(i).getNoteId());
                ps.setString(3, noteComment.get(i).getContent());
                ps.setString(4, TimeUtil.format(noteComment.get(i).getCreateAtUtc()));
                ps.setString(5, TimeUtil.format(noteComment.get(i).getUpdateAtUtc()));
                ps.setBoolean(6, noteComment.get(i).isDeleted());
                ps.setInt(7, noteComment.get(i).getScore());
                ps.setString(8, noteComment.get(i).getUserId());
            }

            @Override
            public int getBatchSize() {
                return noteComment.size();
            }
        });
    }
```

#### 查询


```java
public List<Student> listStudents() {
        String SQL = "select * from Student";
        List<Student> students = jdbcTemplateObject.query(SQL, new StudentMapper());
        return students;
    }
    
public class StudentMapper implements RowMapper<Student> {
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        Student student = new Student();
        student.setId(rs.getInt("id"));
        student.setName(rs.getString("name"));
        student.setAge(rs.getInt("age"));
        return student;
    }
}
```

##### 带参数查询

###### 返回单行单列数据T


- public < T > T queryForObject(String sql, Map<String, ?> paramMap, Class requiredType)
- public < T > T queryForObject(String sql, SqlParameterSource paramSource, Class requiredType)



```java
String name = template.queryForObject( "select name from student where home_address  limit 1 ", EmptySqlParameterSource.INSTANCE, String.class);
```

###### 返回单行数据T：


- public < T> T queryForObject(String sql, Map< String, ?> paramMap, RowMapper< T>rowMapper)
- public < T> T queryForObject(String sql, SqlParameterSource paramSource, RowMapper< T> rowMapper)



```java
Student  stu = template.queryForObject(
                "select * from student limit 1", new HashMap<>(), new BeanPropertyRowMapper<Student>(Student.class));
//BeanPropertyRowMapper会把下划线转化为驼峰属性
//结果对象可比实际返回字段多或者少
//这两个API也可以使用SingleColumnRowMapper返回单行单列数据
String name = template.queryForObject(
                "select name from student limit 1", EmptySqlParameterSource.INSTANCE, new SingleColumnRowMapper<>(String.class));
```

###### 返回Map形式单行数据


- public Map< String, Object> queryForMap(String sql, Map< String, ?> paramMap)
- public Map< String, Object> queryForMap(String sql, SqlParameterSource paramSource)

```java
 Map< String, Object> studentMap = template.queryForMap("select * from student limit 1", new HashMap<>());
```

###### 返回多行单列数据List


- API: public < T> List< T> queryForList(String sql, Map<String, ?> paramMap, Class< T > elementType)
- API: public < T> List< T> queryForList(String sql, SqlParameterSource paramSource, Class< T> elementType)
```java
List< String> namelist = template.queryForList("select name from student", new HashMap<>(), String.class);
```

###### 返回多行数据List


- public < T> List< T> query(String sql, Map< String, ?> paramMap, RowMapper< T> rowMapper)
- public < T> List< T> query(String sql, SqlParameterSource paramSource, RowMapper< T> rowMapper)
- public < T> List< T> query(String sql, RowMapper< T> rowMapper)



```java
sql中参数是问号表达式
public List<NoteCommentScoreModel> getNoteCommentScoreCount(String teacherId, String year, String month, String centerId) {
        try {
            return jdbcTemplate.query(getNoteCommentScoreCountSql, new String[]{teacherId, teacherId, year, month, centerId}, new RowMapper<NoteCommentScoreModel>() {
                @Override
                public NoteCommentScoreModel mapRow(ResultSet resultSet, int i) throws SQLException {
                    NoteCommentScoreModel model = new NoteCommentScoreModel();
                    model.setMonth(resultSet.getString("Month"));
                    model.setNumbers(resultSet.getString("Numbers"));
                    model.setScore(resultSet.getInt("LastScore"));
                    model.setYear(resultSet.getString("Year"));
                    return model;
                }
            });
        } catch (EmptyResultDataAccessException e) {
            return null;
        }
    }
```


**BeanPropertySqlParameterSource,MapSqlParameterSource提供了对SqlParameterSource的实现，**


**允许使用javabean或者Map来实现对命名参数传递。**


```java
// 使用：BeanPropertySqlParameterSource
@Repository
public class CompanyDao {
	private SimpleJdbcInsert insertCompany;
    @Autowired
    public void setDataSource(DataSource dataSource) {
        this.insertCompany = new SimpleJdbcInsert(dataSource).withTableName("company").usingGeneratedKeyColumns("id");
    }
    public void addCompnay(Company comp){
    	SqlParameterSource parameters = new BeanPropertySqlParameterSource(comp);
        Number genId = insertCompany.executeAndReturnKey(parameters);
        comp.setId(genId.longValue());
    }
}
```


```java
//使用MapSqlParameterSource
@Repository
public class CompanyDao {
	private SimpleJdbcInsert insertCompany;
    @Autowired
    public void setDataSource(DataSource dataSource) {
        this.insertCompany = new SimpleJdbcInsert(dataSource).withTableName("company").usingGeneratedKeyColumns("id");
    }
    public void addCompnay(Company comp){
    	SqlParameterSource parameters = new MapSqlParameterSource().addValue("name", comp.getName())
    			.addValue("location",comp.getId()).addValue("no_of_emp",comp.getNoOfEmp());
        Number genId = insertCompany.executeAndReturnKey(parameters);
        comp.setId(genId.longValue());
    }
}
```

###### 返回多行数据 map


- public List< Map< String, Object>> queryForList(String sql, Map< String, ?> paramMap)
- public List< Map< String, Object>> queryForList(String sql, SqlParameterSource paramSource)

```java
public Student getStudent(Integer id) {
        String SQL = "select * from Student where id = ?";
        Student student = jdbcTemplateObject.queryForObject(SQL, new Object[] { id }, new StudentMapper());
        return student;
    }
public class StudentMapper implements RowMapper<Student> {
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        Student student = new Student();
        student.setId(rs.getInt("id")); // 列名
        student.setName(rs.getString("name"));
        student.setAge(rs.getInt("age"));
        return student;
    }
}
```


在我所在的项目中日常带参数查询有以下方式进行操作：


```java
// 通过替换参数
public static final String SQL_GET_PARENT_PUSH_SWITCH = "SELECT MetaValue,Id FROM users_metadata WHERE Id IN ({parentIds}) AND MetaKey = ?";
public List<PushParentSwitch> getAllParentPushSwitch(String parentIds, String type) {
        String sql = UserMapper.SQL_GET_PARENT_PUSH_SWITCH.replace("{parentIds}",parentIds);
        return jdbcTemplate.query(sql, new String[]{type},new RowMapper<PushParentSwitch>() {
            @Override
            public PushParentSwitch mapRow(ResultSet rs, int i) throws SQLException {
                PushParentSwitch push = new PushParentSwitch();
                push.setId(rs.getString("Id"));
                push.setMetaValue(rs.getString("MetaValue"));
                return push;
            }
        });
    }
/////////////通过值映射赋值参数
    private final static String getDomainByGroupIdSql = "SELECT dd.* FROM contents cg LEFT JOIN domains dd \n" +
            "ON cg.ClassDomainId=dd.Id \n" +
            "WHERE cg.Id=? AND \n" +
            "dd.IsDeleted=0";
 public DomainEntity getDomainByGroupId(String groupId) {
        try {
            return jdbcTemplate.queryForObject(getDomainByGroupIdSql, new Object[]{groupId}, DomainMapper.MAPPER_DOMAIN);
        } catch (EmptyResultDataAccessException e) {
            return null;
        }
    }
```

###### in查询：NamedParameterJdbcTemplate


通过NamedParameterJdbcTemplate 设置参数主要是in查询.如果我们在sql中使用了in，那么通过?占位符来传参是不能解决问题的，直接拼接sql又会有sql注入的风险。这种情况下我们可以使用NamedParameterJdbcTemplate 来解决问题。


```java
///通过NamedParameterJdbcTemplate 设置参数主要是in查询
//如果我们在sql中使用了in，那么通过?占位符来传参是不能解决问题的，直接拼接sql又会有sql注入的风险。这种情况下我们可以使用NamedParameterJdbcTemplate 来解决问题。
//NamedParameterJdbcTemplate支持具名参数
 public static final String SQL_GET_REVIEW_NOTES_BYIDS_V2="SELECT DISTINCT n.*,g.DomainId GroupDomainId from notes_note n\n" +
            "\tLEFT JOIN notes_noteenrollment ne on ne.NoteId = n.Id\n" +
            "\tLEFT JOIN contents_enrollment e on e.Id = ne.EnrollmentId\n" +
            "\tLEFT JOIN enrollment_period p on p.EnrollmentId = e.Id\n" +
            "\tLEFT JOIN contents_group g on g.Id = e.GroupId\n" +
            "\tLEFT JOIN contents_center c on c.Id = g.CenterId\n"+
            "\twhere g.IsDeleted = 0 AND g.IsInactive = 0\n" +
            "\tAND e.IsDeleted = 0 AND e.IsInactive = 0 AND n.IsDeleted = 0\n" +

            "\tAND n.Type = 'Normal'\n" +
            "\tAND (n.IsHidden = 0 OR n.IsHidden IS NULL)\n" +
            "\tAND n.CreateAtLocal >= CONVERT(char(10), p.FromAtLocal, 110)\n" +
            "\tAND n.CreateAtLocal <= CONVERT(char(10), p.ToAtLocal, 110) + ' 23:59:59'\n" +
            "\tAND n.Id IN (:noteIds)\n";
public List<com.learninggenie.common.data.entity.NoteEntity> getReviewNotesByIds(List<String> noteIds,String oldSql) {
        String sql = NoteMapper.SQL_GET_REVIEW_NOTES_BYIDS_V2;
        NamedParameterJdbcTemplate namedParameterJdbcTemplate=new NamedParameterJdbcTemplate(jdbcTemplate);
        Map<String,Object> params=new HashMap<>();
        params.put("noteIds",noteIds);
        return namedParameterJdbcTemplate.query(sql+oldSql, params, NoteMapper.NOTE_ENTITY_ROW_MAPPER);

    }

@Override
	public List<Item> selectItemByIds(String itemIds) {
		NamedParameterJdbcTemplate nameJdbc = new NamedParameterJdbcTemplate(jdbcTemplate);
		Map<String,Object> paramMap = new HashMap<String, Object>();
		try {
			String sql = "SELECT  *  FROM zcy_goods_item WHERE id IN(:itemIds) ORDER BY id DESC";
			paramMap.put("itemIds", Arrays.asList(itemIds.split(",")));
			return nameJdbc.query(sql,paramMap, new BeanPropertyRowMapper<Item>(Item.class));
		} catch (DataAccessException e) {
			return null;
		}
	}
```


具体的NamedParameter参考https://blog.csdn.net/u011179993/article/details/74791304


通过查看源码，无论是queryfor*还是query本身都是走的底层query方法


```java
 public <T> T query(final String sql, final ResultSetExtractor<T> rse) throws DataAccessException {
        Assert.notNull(sql, "SQL must not be null");
        Assert.notNull(rse, "ResultSetExtractor must not be null");
        if (this.logger.isDebugEnabled()) {
            this.logger.debug("Executing SQL query [" + sql + "]");
        }

        class QueryStatementCallback implements StatementCallback<T>, SqlProvider {
            QueryStatementCallback() {
            }

            public T doInStatement(Statement stmt) throws SQLException {
                ResultSet rs = null;

                Object var4;
                try {
                    rs = stmt.executeQuery(sql);
                    ResultSet rsToUse = rs;
                    if (JdbcTemplate.this.nativeJdbcExtractor != null) {
                        rsToUse = JdbcTemplate.this.nativeJdbcExtractor.getNativeResultSet(rs);
                    }

                    var4 = rse.extractData(rsToUse);
                } finally {
                    JdbcUtils.closeResultSet(rs);
                }

                return var4;
            }

            public String getSql() {
                return sql;
            }
        }

        return this.execute((StatementCallback)(new QueryStatementCallback()));
    }
public <T> T query(PreparedStatementCreator psc, final PreparedStatementSetter pss, final ResultSetExtractor<T> rse) throws DataAccessException {
        Assert.notNull(rse, "ResultSetExtractor must not be null");
        this.logger.debug("Executing prepared SQL query");
        return this.execute(psc, new PreparedStatementCallback<T>() {
            public T doInPreparedStatement(PreparedStatement ps) throws SQLException {
                ResultSet rs = null;

                Object var4;
                try {
                    if (pss != null) {
                        pss.setValues(ps);
                    }

                    rs = ps.executeQuery();
                    ResultSet rsToUse = rs;
                    if (JdbcTemplate.this.nativeJdbcExtractor != null) {
                        rsToUse = JdbcTemplate.this.nativeJdbcExtractor.getNativeResultSet(rs);
                    }

                    var4 = rse.extractData(rsToUse);
                } finally {
                    JdbcUtils.closeResultSet(rs);
                    if (pss instanceof ParameterDisposer) {
                        ((ParameterDisposer)pss).cleanupParameters();
                    }

                }

                return var4;
            }
        });
    }
```

#### 更新


API:


- int update(String sql, Map<String, ?> paramMap)
- int update(String sql, SqlParameterSource paramSource)
- int update(String sql, SqlParameterSource paramSource)

```java
String updateQuery = "update Student set age = ? where id = ?";
jdbcTemplateObject.update(updateQuery, age, id);
```

##### 批量更新


```java
String SQL = "update Student set age = ? where id = ?";
int[] updateCounts = jdbcTemplateObject.batchUpdate(SQL,
   new BatchPreparedStatementSetter() {
      public void setValues(PreparedStatement ps, int i) throws SQLException {
         ps.setInt(1, students.get(i).getAge());                        
         ps.setInt(2, students.get(i).getId());    
      }
      public int getBatchSize() {
         return students.size();
      }
   });
```

#### 删除


```java
String deleteQuery = "delete from Student where id = ?";
jdbcTemplateObject.update(deleteQuery, id);
```

#### RowMapper


JdbcTemplate类使用org.springframework.jdbc.core.RowMapper 接口在每行的基础上映射ResultSet的行。该接口的实现执行将每行映射到结果对象的实际工作。如果抛出SQLExceptions将被调用的JdbcTemplate捕获和处理。

接口的声明以下是org.springframework.jdbc.core.RowMapper接口的声明 -

public interface RowMapper

用法


- 步骤1 - 使用配置的数据源创建一个JdbcTemplate对象。
- 步骤2 - 创建一个实现RowMapper接口的StudentMapper对象。
- 步骤3 - 使用JdbcTemplate对象方法在使用StudentMapper对象时进行数据库操作

```java
public void insertMediaAll(MediaEntity media){
        jdbcTemplate.update(MediaMapper.insertMediaAll, media.getId(), media.getRelativePath(), media.getCreateAtUtc(),media.getSnapshotPath()
                ,media.getMimeType(),media.getWidth(),media.getHeight(),media.getSize(),media.getFileType(),media.getFileName(),media.getAnnexType(),media.getVoiceTime());
    }
//MediaMapper下的
    public static final String insertMediaAll = "insert into medias_media(Id,RelativePath,CreateAtUtc,SnapshotPath,MimeType,Width,Height,Size,FileType,FileName,AnnexType,VoiceTime) VALUES (?,?,?,?,?,?,?,?,?,?,?,?)";

// 结果集映射 查询的时候 query
 @Override
    public List<MediaResourceEntity> getMediaResourceByPlayIds(String playIds) {
        String sql = MediaMapper.SQL_SELECT_MEDIA_RESOURE_PLAYIDS.replace("@ids", playIds);
        return jdbcTemplate.query(sql, MediaMapper.MAPPER_MEDIAS_RESOURCE);
    }

//region mapper 映射的列
    public static final RowMapper<MediaResourceEntity> MAPPER_MEDIAS_RESOURCE = new RowMapper<MediaResourceEntity>() {
        @Override
        public MediaResourceEntity mapRow(ResultSet resultSet, int i) throws SQLException {
            MediaResourceEntity mediaResource = new MediaResourceEntity();
            mediaResource.setId(DBUtil.getString(resultSet, ID));
            mediaResource.setPlaylistId(DBUtil.getString(resultSet, PLAYLISTID));
            mediaResource.setCategory(DBUtil.getString(resultSet, CATEGORY));
            mediaResource.setLevel(DBUtil.getString(resultSet, LEVEL));
            mediaResource.setSource(DBUtil.getString(resultSet, SOURCE));
            mediaResource.setCreateAtUtc(DBUtil.getDate(resultSet, CREATE_AT_UTC));
            mediaResource.setDeleted(DBUtil.getBoolean(resultSet, IS_DELETED));
            mediaResource.setSortIndex(DBUtil.getInt(resultSet, SORTINDEX));
            mediaResource.setImg(DBUtil.getString(resultSet, IMG));
            mediaResource.setNeedPaid(DBUtil.getBoolean(resultSet, NEEDPAID));
            mediaResource.setPlayListName(DBUtil.getString(resultSet, PLAYLISTNAME));
            return mediaResource;
        }
    };
// queryforobject
 private static final String getSubTopicByNameSql = "select top 1 * from medias_subtopic where Name = ? and ParentId = ?";
    @Override
    public TopicEntity getSubTopicByNameAndParent(String name, String parentId) {
        try {
            return jdbcTemplate.queryForObject(getSubTopicByNameSql, new String[]{name, parentId}, new RowMapper<TopicEntity>() {
                @Override
                public TopicEntity mapRow(ResultSet rs, int rowNum) throws SQLException {
                    TopicEntity topic = new TopicEntity();
                    topic.setId(DBUtil.getString(rs, "Id"));
                    topic.setName(DBUtil.getString(rs, "Name"));
                    topic.setParentId(DBUtil.getString(rs, "ParentId"));
                    return topic;
                }
            });
        } catch (EmptyResultDataAccessException e) {
            return null;
        }
    }
```

#### 查询参数设置值：PreparedStatementSetter


此接口为 JdbcTemplate 类提供的 PreparedStatement 设置值，该值用于使用相同 SQL 的批处理中的每个更新。 实现负责设置任何必要的参数。 已经提供了带占位符的 SQL。使用这个接口比使用 PreparedStatementCreator 更容易: JdbcTemplate 将创建 PreparedStatement，回调只负责设置参数值。实现不需要关注它们尝试的操作可能抛出的 SQLExceptions。 Jdbctemplate 类将适当地捕获和处理 SQLExceptions。

| `void` | `setValues(PreparedStatement ps)`Set parameter values on the given PreparedStatement. 在给定的 PreparedStatement 上设置参数值 |
| --- | --- |
|  |  |

```java
final String SQL = "select * from Student where id = ? ";
List <Student> students = jdbcTemplateObject.query(SQL, new PreparedStatementSetter() {   
    public void setValues(PreparedStatement preparedStatement) throws SQLException {      preparedStatement.setInt(1, id);  } }, new RowMapper<TopicEntity>() {
                @Override
                public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
                  Student student = new Student();
                  student.setId(rs.getInt("id"));
                  student.setName(rs.getString("name"));
                  student.setAge(rs.getInt("age"));
                  return student;
               }
            });
        } catch (EmptyResultDataAccessException e) {
            return null;}
        });
```


在以下类型中处理似乎开发中还未遇见。

#### JDBC处理Blob类型


```java
public class StudentJDBCTemplate implements StudentDAO {
   private DataSource dataSource;
   private JdbcTemplate jdbcTemplateObject;

   public void setDataSource(DataSource dataSource) {
      this.dataSource = dataSource;
   }

   public void updateImage(Integer id, byte[] imageData) {
      MapSqlParameterSource in = new MapSqlParameterSource();
      in.addValue("id", id);
      in.addValue("image",  new SqlLobValue(new ByteArrayInputStream(imageData), 
         imageData.length, new DefaultLobHandler()), Types.BLOB);

      String SQL = "update Student set image = :image where id = :id";

      NamedParameterJdbcTemplate jdbcTemplateObject = new NamedParameterJdbcTemplate(dataSource);
      jdbcTemplateObject.update(SQL, in);
      System.out.println("Updated Record with ID = " + id );
   }
}
```

#### JDBCTemplate处理clob类型字段


```java
public class StudentJDBCTemplate implements StudentDAO {
   private DataSource dataSource;
   private JdbcTemplate jdbcTemplateObject;

   public void setDataSource(DataSource dataSource) {
      this.dataSource = dataSource;
   }

   public void updateImage(Integer id, byte[] imageData) {
      MapSqlParameterSource in = new MapSqlParameterSource();
      in.addValue("id", id);
      in.addValue("image",  new SqlLobValue(new ByteArrayInputStream(imageData), 
         imageData.length, new DefaultLobHandler()), Types.BLOB);

      String SQL = "update Student set image = :image where id = :id";

      NamedParameterJdbcTemplate jdbcTemplateObject = new NamedParameterJdbcTemplate(dataSource);
      jdbcTemplateObject.update(SQL, in);
      System.out.println("Updated Record with ID = " + id );
   }
}
```
