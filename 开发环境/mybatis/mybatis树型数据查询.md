一、表结构  

```
CREATE TABLE `my_tree` (
	`test_id` VARCHAR(32) NOT NULL,
	`gjfl_name` VARCHAR(100) NULL DEFAULT NULL,
	`parent_id` VARCHAR(32) NULL DEFAULT NULL,
	`gjfl_level` INT(11) NULL DEFAULT NULL,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;
```
二、构建数据  

```
INSERT INTO `my_tree` VALUES ('1', '1', '0', '1');
INSERT INTO `my_tree` VALUES ('2', '2', '1', '2');
INSERT INTO `my_tree` VALUES ('3', '3', '0', '1');
INSERT INTO `my_tree` VALUES ('4', '4', '3', '2');
INSERT INTO `my_tree` VALUES ('5', '5', '4', '3');
INSERT INTO `my_tree` VALUES ('6', '6', '2', '3');
INSERT INTO `my_tree` VALUES ('7', '7', '3', '2');
```
三、实体  

```
@Table(name = "my_tree")
public class MyTree implements Serializable {
    @Id
    @Column(name = "test_id")
    private String testId;

    @Column(name = "gjfl_name")
    private String gjflName;

    @Column(name = "parent_id")
    private String parentId;

    @Column(name = "gjfl_level")
    private Integer gjflLevel;

    private List<MyTree> childrenList;//注意这里
//忽略setter、getter方法，使用lombok可以生成setter、getter
}
```
四、mapper配置  
**注意collection里的配置：javaType、 column、select**  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test.dao.MyTreeMappper">
<resultMap id="testMap" type="com.test.model.MyTree">
    <!--
      WARNING - @mbg.generated
    -->
    <id column="test_id" jdbcType="VARCHAR" property="testId" />
    <result column="gjfl_name" jdbcType="VARCHAR" property="gjflName" />
    <result column="parent_id" jdbcType="VARCHAR" property="parentId" />
    <result column="gjfl_level" jdbcType="INTEGER" property="gjflLevel" />
  </resultMap>
  <resultMap id="SubPrivilegesResult" type="com.yinghu.app.rds.monthlyreport.test.model.Gjfl" extends="testMap">
 <!-- javaType、 column、select是关键参数 -->
    <collection property="childrenList" javaType="java.util.ArrayList" column="test_id"
                ofType="com.yinghu.app.rds.monthlyreport.test.model.Gjfl" select="findLevel">
      <!--<id column="id" jdbcType="VARCHAR" property="id" />-->
      <!--<result column="gjfl_name" jdbcType="VARCHAR" property="gjflName" />-->
      <!--<result column="parent_id" jdbcType="VARCHAR" property="parentId" />-->
      <!--<result column="gjfl_level" jdbcType="INTEGER" property="gjflLevel" />-->
    </collection>
  </resultMap>
  <select id="findLevel" resultMap="SubPrivilegesResult">
    SELECT * FROM my_tree where parent_id = #{testId} <--这里是父级ID-->
  </select>
</mapper>
```
五、DAO层  

```

public interface MyTreeMapper {

        List<MyTree> findLevel(String param);

}
```
service及controller层与平常没有差别  

结果  

```
"list":[
	{"testId":"1","gjflName":"1","parentId":"0","gjflLevel":1,
		"childrenList":[
			{"testId":"2","gjflName":"2","parentId":"1","gjflLevel":2,
				"childrenList":[
					{"testId":"6","gjflName":"6","parentId":"2","gjflLevel":3,"childrenList":[]}
				]
			}
		]
	},
	{"testId":"3","gjflName":"3","parentId":"0","gjflLevel":1,
		"childrenList":[
			{"testId":"4","gjflName":"4","parentId":"3","gjflLevel":2,
				"childrenList":[
					{"testId":"5","gjflName":"5","parentId":"4","gjflLevel":3,"childrenList":[]}
				]
			},
			{"testId":"7","gjflName":"7","parentId":"3","gjflLevel":2,"childrenList":[]}
		]
	}
]
```
