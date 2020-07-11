---
title: jpa使用相关
date: 2020-07-06 20:41:03
categories:
- java
---

#### pom.xml

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

#### 基础用法

https://www.cnblogs.com/timeout/p/11084382.html

1. Spring Data JPA为自定义查询提供了一些表达条件查询的关键字，大致如下：

- And --- 等价于 SQL 中的 and 关键字，比如 findByUsernameAndPassword(String user, Striang pwd)；
- Or --- 等价于 SQL 中的 or 关键字，比如 findByUsernameOrAddress(String user, String addr)；
- Between --- 等价于 SQL 中的 between 关键字，比如 findBySalaryBetween(int max, int min)；
- LessThan --- 等价于 SQL 中的 "<"，比如 findBySalaryLessThan(int max)；
- GreaterThan --- 等价于 SQL 中的">"，比如 findBySalaryGreaterThan(int min)；
- IsNull --- 等价于 SQL 中的 "is null"，比如 findByUsernameIsNull()；
- IsNotNull --- 等价于 SQL 中的 "is not null"，比如 findByUsernameIsNotNull()；
- NotNull --- 与 IsNotNull 等价；
- Like --- 等价于 SQL 中的 "like"，比如 findByUsernameLike(String user)；
- NotLike --- 等价于 SQL 中的 "not like"，比如 findByUsernameNotLike(String user)；
- OrderBy --- 等价于 SQL 中的 "order by"，比如 findByUsernameOrderBySalaryAsc(String user)；
- Not --- 等价于 SQL 中的 "！ ="，比如 findByUsernameNot(String user)；
- In --- 等价于 SQL 中的 "in"，比如 findByUsernameIn(Collection userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；
- NotIn --- 等价于 SQL 中的 "not in"，比如 findByUsernameNotIn(Collection userList) ，方法的参数可以是 Collection 类型，也可以是数组或者不定长参数；

2. @Query注解的使用非常简单，只需在声明的方法上面标注该注解，同时提供一个JP QL查询语句即可

   ```java
   public interface UserDao extends Repository<AccountInfo, Long> { 
    
   	@Query("select a from AccountInfo a where a.accountId = ?1") 
   	AccountInfo findByAccountId(Long accountId); 
   	 
   	   @Query("select a from AccountInfo a where a.balance > ?1") 
   	Page<AccountInfo> findByBalanceGreaterThan( 
   	Integer balance,Pageable pageable); 
   }
   ```

3. https://www.cnblogs.com/wanthune/p/12666657.html

   使用json类型

   ```xml
   <dependency>
      <groupId>com.vladmihalcea</groupId>
      <artifactId>hibernate-types-52</artifactId>
      <version>2.4.3</version>
   </dependency>
   ```

   ```java
   @Data
   @Entity
   @TypeDef(name = "json", typeClass = JsonStringType.class)
   public class ExpressOrder{
       /**主键自增 */
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
    
       /**商品相关信息 */
       @Type(type = "json")
       @Column(columnDefinition = "json" )
       private List<CargoModel> cargoModelList;
    
       /**增值服务信息 */
       @Type(type = "json")
       @Column(columnDefinition = "json" )
       private List<AddedServiceModel> addedServiceModelList;
   }
   ```

#### Model定义

application:

```java
@EntityScan({"com.task"}) //扫描加@Entity 注解的类
public class TaskApplication {
  
}
```

application.yml:

```yml
#驼峰和下划线转换设置：org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy
spring:
  jpa:
      hibernate:
        ddl-auto: update
        naming:
          physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl #字段名不进行驼峰和下划线转换
      properties:
        hibernate:
          format_sql: true
          show_sql: false
          dialect: org.hibernate.dialect.MySQL5InnoDBDialect
```

索引：

```java
@Table(indexes = {@Index(name = "idx_code", columnList = "code")})
```

字段定义：

```java
@Entity
@Data
@JsonIgnoreProperties(value={"hibernateLazyInitializer","handler","fieldHandler"})
public class Task {
  	@Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    Long id;
    @Column(columnDefinition = "varchar(64)",unique = true)
    String code;
}

# @Column属性详解：

name
定义了被标注字段在数据库表中所对应字段的名称；

unique
表示该字段是否为唯一标识，默认为false。如果表中有一个字段需要唯一标识，则既可以使用该标记，也可以使用@Table标记中的@UniqueConstraint。

nullable
表示该字段是否可以为null值，默认为true。

insertable
表示在使用“INSERT”脚本插入数据时，是否需要插入该字段的值。

updatable
表示在使用“UPDATE”脚本插入数据时，是否需要更新该字段的值。insertable和updatable属性一般多用于只读的属性，例如主键和外键等。这些字段的值通常是自动生成的。

columnDefinition（大多数情况，几乎不用）
表示创建表时，该字段创建的SQL语句，一般用于通过Entity生成表定义时使用。（也就是说，如果DB中表已经建好，该属性没有必要使用。）

table
表示当映射多个表时，指定表的表中的字段。默认值为主表的表名。

length
表示字段的长度，当字段的类型为varchar时，该属性才有效，默认为255个字符。

precision和scale
precision属性和scale属性表示精度，当字段类型为double时，precision表示数值的总长度，scale表示小数点所占的位数。
```



#### 迁移

1. 版本控制表

   ```java
   @Data
   @Table
   @Entity
   @JsonIgnoreProperties(value={"hibernateLazyInitializer","handler","fieldHandler"})
   public class MigrationVersion {
       @Id
       @GeneratedValue(strategy = GenerationType.AUTO)
       Long id;
       String klass;
       long batchNo;
       int version;
       Date createdAt;
       short done=0;
   }
   ```



#### 问题

* 数据库中自动生成hibernate_sequence表。

  ```java
  //指定主键使用自增策略 GenerationType.IDENTITY
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
  ```

  