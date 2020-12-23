# Mybatis-查询语句中if，choose,SQL代码片段等使用

## 	if语句的使用

```xml
 <!--通过if查询-->
    <select id="queryBlogIF" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <if test="title != null">
                and title = #{title}
            </if>
            <if test="author != null">
                and author = #{author}
            </if>
        </where>

    </select>
```

## 	choose语句的使用(相当于which和case只能选择其中一个)

```xml
<!--通过choose选择-->
    <select id="queryBlogChoose" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <choose>
                <when test="title != null">
                    and title = #{title}
                </when>
                <when test="author != null">
                    and author = #{author}
                </when>
                <otherwise>
                    and views = #{views}
                </otherwise>
            </choose>
        </where>
    </select>
```

​	SQL片段

```xml
 <sql id="if-title-author">
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author},
        </if>
    </sql>
```

```xml
 <update id="updateBlog" parameterType="map">
        update blog
        <set>
            <include refid="if-title-author"></include>
        </set>
        where id = #{id}
    </update>
```

注意事项

- 最好是单表查询
- 不要包含where标签



### 	foreach的使用

```xml
<!--
        我们现在传递一个map，这个map中可以存在一个集合
        每个遍历的对象为id，以and（为开头，）为结尾，中间用or分开，拼接成一个mysql语句
        select * from blog WHERE ( id = ? or id = ? or id = ? ) 
    -->
    <select id="queryBlogForeach" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <foreach collection="ids" item="id" open="and (" close=")" separator="or">
                id = #{id}
            </foreach>
        </where>
    </select>
```





