## MyBatis-Plus

### 1.简介
1. [MyBatis-Plus](https://mybatis.plus/)(简称MP)是一个MyBatis的增强工具, 在MyBatis的基础上只做增强不做改变, **为简化开发、提高效率而生**
2. 引入MP工具不会对原有MyBatis功能逻辑产生影响, 配合其丰富的插件还会极大增强单独使用MyBatis的功能. 如同其官网介绍的一样
	1. 润物无声. 只做增强不做改变, 引入它不会对现有工程产生影响, 如丝般顺滑
	2. 效率至上. 只需简单配置, 即可快速进行 CRUD 操作, 从而节省大量时间
	3. 丰富功能. 热加载、代码生成、分页、性能分析等功能一应俱全

### 2.基本CRUD

下面先通过下面的示例直观感受下其强大. 假设我们有一个user表, 建表语句以及初始数据如下

```
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	gmt_create datetime not null default current_timestamp comment '创建时间',
	gmt_modified datetime not null default current_timestamp comment '修改时间',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);

DELETE FROM user;

INSERT INTO user (id, gmt_create, gmt_modified, name, age, email) VALUES
(1, now(), now(), 'foo', 18, 'foo@alibaba-inc.com'),
(2, now(), now(), 'bar', 20, 'bar@cainiao-inc.com'),
(3, now(), now(), 'sx', 30, 'sx@alibaba-inc.com'),
(4, now(), now(), 'jz', 33, 'jz@cainiao-inc.com'),
(5, now(), now(), 'rj', 28, 'rj@cainiao-inc.com');
```
对应的Java DO 如下

```java
@Data
@Accessors(chain = true)
public class User {

    private Long id;

    private LocalDateTime gmtCreate;

    private LocalDateTime gmtModified;

    private String name;

    private Integer age;

    private String email;
}
```
MP中的Mapper只用继承com.baomidou.mybatisplus.core.mapper.BaseMapper即可完成大部分的CRUD操作, 满足大部分使用场景

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```
从下面代码可以看出基本CRUD的写法类似于通用Mapper, 但个人感觉写起来比通用Mapper要爽很多, 尤其是条件构造器Wrapper部分, 支持lambda的语法写起来很清爽, 读起来也顺畅

#### 1.Insert

```java
@Test
public void testInsert() {
    User user = new User();
    user.setName("test");
    user.setAge(30);
    user.setEmail("test@alibaba-inc.com");
    assertThat(mapper.insert(user)).isGreaterThan(0);
    // 默认获取插入后id
    assertThat(user.getId()).isNotNull();
}
```
只用调用baseMapper中已有的insert方法即可完成数据的插入, 并且会自动获得插入后的记录主键id. MP支持多种主键策略, 有自增、用户输入、全局Long型唯一ID、UUID等

#### 2.Update

```java
User entity = new User().setId(1L).setEmail("test@cainiao-inc.com");
assertThat(mapper.updateById(entity)).isGreaterThan(0);
assertThat(mapper.selectById(1).getEmail()).isEqualTo("test@cainiao-inc.com");

mapper.update(
    new User().setEmail("test@alibaba-inc.com"),
    new QueryWrapper<User>()
        .lambda().eq(User::getName, "rj")
);
user = mapper.selectById(5);
assertThat(user.getEmail()).isEqualTo("test@alibaba-inc.com");
```
基础mapper中提供了基本的updateById(T entity)方法, 也提供了根据条件修改的update(T entity, Wrapper<T> updateWrapper)方法. MP的条件构造器Wrapper功能很强大, 使用它可以构造出能满足大多数场景的WHERE条件, 后面会详细介绍Wrapper的使用方法

#### 3.Delete

```java
assertThat(mapper.deleteById(3L)).isGreaterThan(0);
assertThat(mapper.deleteBatchIds(List.of(4, 5))).isGreaterThan(0);
assertThat(mapper.delete(new LambdaQueryWrapper<User>()
    .eq(User::getName, "foo"))).isGreaterThan(0); 
```
同样的, 基础mapper中提供了基本的deleteById/deleteBatchIds方法, 也提供了根据条件删除的delete(Wrapper<T> updateWrapper)方法

#### 4.Select

```java
mapper.insert(
            new User()
            .setId(1024L)
            .setName("foo")
            .setEmail("foo@alibaba-inc.com")
            .setAge(30));

assertThat(mapper.selectById(1024L).getEmail()).isEqualTo("foo@alibaba-inc.com");


List<User> users = mapper.selectList(Wrappers.<User>lambdaQuery().like(User::getEmail, "cainiao-inc"));
assertThat(users.size()).isEqualTo(3);

```
基础mapper中提供了基本的selectById方法, 以及根据条件查询的selectList方法

#### 5.基本原理
简要看了下MP的源码以及执行流程, MP会为Mapper生成代理MyBatisMapperProxy, 在启动的时候将Mapper接口中的方法及其对应的SQL文件处理好, 通过MapperBuilderAssistant保存在SqlSessionFactory->configuration对象的mappedStatements中, 启动完成后即可直接使用了. 基本的SQL语句可以参考SqlMethod枚举中定义的方法. 简单摘要列一下

```
/**
 * 插入
 */
INSERT_ONE("insert", "插入一条数据（选择字段插入）", "<script>\nINSERT INTO %s %s VALUES %s\n</script>"),

/**
 * 删除
 */
DELETE_BY_ID("deleteById", "根据ID 删除一条数据", "<script>\nDELETE FROM %s WHERE %s=#{%s}\n</script>"),

/**
 * 逻辑删除
 */
LOGIC_DELETE_BY_ID("deleteById", "根据ID 逻辑删除一条数据", "<script>\nUPDATE %s %s WHERE %s=#{%s} %s\n</script>"),

/**
 * 修改
 */
UPDATE_BY_ID("updateById", "根据ID 选择修改数据", "<script>\nUPDATE %s %s WHERE %s=#{%s} %s\n</script>"),


/**
 * 查询
 */
SELECT_BY_ID("selectById", "根据ID 查询一条数据", "SELECT %s FROM %s WHERE %s=#{%s}"),

/**
 * 逻辑删除 -> 查询
 */
LOGIC_SELECT_BY_ID("selectById", "根据ID 查询一条数据", "SELECT %s FROM %s WHERE %s=#{%s} %s"),

```	

### 3.条件构造器AbstractWrapper
从上面的示例中可以看到, MP除了提供基本的insert/deleteById/selectById/updateById等基本接口之外, 还提供了根据条件构造器AbstractWrapper进行delete/select/update的通用接口. MP的条件构造器很强大, 通过它能构造出日常工作中遇到的大部分场景. 下面举几个简单例子说明下其使用, 详细使用参考其[官网](https://mybatis.plus/guide/wrapper.html#abstractwrapper)

#### 1. eq

```
eq(R column, Object val)

例子: eq("name", "老王") ---> name = '老王'
```
#### 2. between

```
between(R column, Object val1, Object val2)

例子: between("age", 18, 30) ---> age between 18 and 30
```
#### 3. like

```
like(R column, Object val)

例子: like("name", "王") ---> name like '%王%'
```
#### 4. in

```
in(R column, Collection<?> value)

例子: in("age",{1,2,3}) ---> age in (1,2,3)
```
#### 5. groupBy

```
groupBy(R... columns)

例子: groupBy("id", "name")--->group by id,name
```
#### 6. orderBy

```
orderBy(boolean condition, boolean isAsc, R... columns)

例子: orderBy(true, true, "id", "name")--->order by id ASC,name ASC
```


### 4.插件扩展
除了支持基础常用的CRUD操作, 针对日常开发中经常碰到的分页、逻辑删除、乐观锁、执行分析等场景, MP提供了丰富的插件来极大简化我们的开发, 提升效率. 下面简要介绍两款常用插件: 分页插件+逻辑删除插件, 使用详情以及其他插件详见[官网](https://mybatis.plus/guide/hot-loading.html)

#### 1.分页插件
日常开发中分页功能比较常见, 其执行逻辑基本都是首先根据查询条件进行count获取总量, 再根据查询条件进行select limit分页查询明细. 使用MP的分页插件后就可以消除类似以上逻辑的样板代码, 提升开发效率.

配置

```java
@Bean
public PaginationInterceptor paginationInterceptor() {
    return new PaginationInterceptor();
}
```
使用

```java
@Test
public void testSelectPage() {
    Page<User> page = new Page<>(1, 2);
    IPage<User> pageResult = mapper.selectPage(page,
        Wrappers.<User>lambdaQuery().like(User::getEmail, "cainiao-inc"));
    assertThat(pageResult).isSameAs(page);

    assertThat(pageResult.getRecords()).isNotEmpty();
    assertThat(pageResult.getCurrent()).isEqualTo(1);
    assertThat(pageResult.getSize()).isEqualTo(2);
    assertThat(pageResult.getTotal()).isEqualTo(3);

    pageResult.getRecords().forEach(System.out::println);
}
```
可以看出只需要执行selectPage, 就便面了我们首先执行count逻辑, 然后执行select逻辑之类的样板代码了. 分页相关字段可以直接从Page对象中取出

#### 2.逻辑删除插件
大数据时代的删除功能大部分都是逻辑删除, 即在DB层设置类似delete_flag之类的字段标识是否删除. 为了支持这样的需求, 几乎需要在所有的SQL语句中增加类似 ```WHERE delete_flag = 0```的条件, 如果忘记加条件可能还会导致逻辑错误, 引发故障. 使用MP的逻辑删除插件, 可以帮我们解决这一问题

配置

```
public class User {
	@TableLogic
	private Integer deleteFlag;
}
```

只需要在Model对应的属性上增加@TableLogic注解标识即可

配置完成后的效果: 使用mp自带方法删除和查找都会附带逻辑删除功能(自己写的xml不会)

```
删除时 update user set delete_flag = 1 where id = 1 and delete_flag = 0
查找时 select * from user where delete_flag = 0
```

### 5.总结
MyBatis-Plus工具可以极大较少日常工作中"烦人的"通用SQL逻辑编码工作, 如果逻辑不复杂是可以做到无XML/SQL出现的. 用其官网总结的愿景来做个结束吧:
**我们的愿景是成为 MyBatis 最好的搭档, 就像 魂斗罗 中的 1P、2P, 基友搭配, 效率翻倍**