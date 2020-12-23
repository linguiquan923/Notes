# Mybatis学习日记

## 	2020.10.27

### 	如何使用mybatis的一对多和多对一

​	1、首先在一个项目引入mybati-config.xml和Myutil

​	2、编写实体类，再编写相对应的映射*Mapper.xml文件，要把映射文件在myabtis-config.xml中注入

​	3、关于映射文件中一对多和多对一的使用

- #### 		使用多对一

  - ```xml
    <mapper namespace="com.atlgq.dao.StudentMapper">
        <!--
            1.查询所有的学生
            2.根据查询的tid查询所有的老师,子查询,按照嵌套查询处理
        -->
    
        <!--
            结果集映射
            id对应的是下面select语句查询的resultMap对应的方法
            type对应的是
            -->
        <select id="getStudent" resultMap="StudentTeacher">
            select * from student
        </select>
    
        <resultMap id="StudentTeacher" type="Student">
            <!--
                复杂的属性我们需要单独处理
                对象是用association
                集合是用collection；property是指实体里面的属性名，column是指数据库的属性名，javaType指查询出来的对象类型，select指通过哪个查询语句获取对象
                -->
            <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
        </resultMap>
    
        <select id="getTeacher" resultType="Teacher">
            select * from teacher where id = #{id}
        </select>
    
        <!--按照结果嵌套处理-->
    
        <select id="getStudent2" resultMap="StudentTeacher2">
            select s.id sid,s.name sname,t.name tname
            from student s,teacher t
            where s.tid = t.id
        </select>
        <!--先查询到sid，sname和tname，然后通过tname去获取teacher对象-->
        <resultMap id="StudentTeacher2" type="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <!--查询到的teacher是teacher类型的，将类型定义为Teacher,然后在里面通过查询到的tname和teacher表格里面的name匹配从而获取到老师的信息-->
            <association property="teacher" javaType="Teacher">
                <result property="name" column="tname"/>
            </association>
        </resultMap>
    
    </mapper>
    ```

- #### 使用一对多

  - ```xml
    <mapper namespace="com.atlgq.dao.TeacherMapper">
        <!--按照结果嵌套查询-->
        <select id="getTeacher2" resultMap="TeacherStudent">
            select s.id sid,s.name sname,s.tid tid,t.name tname
            from student s,teacher t
            where s.tid = t.id and t.id = #{tid}
        </select>
        <resultMap id="TeacherStudent" type="Teacher">
            <!--property对应实体类，colunm对应数据库-->
            <result property="id" column="tid"/>
            <result property="name" column="tname"/>
            <!--集合采用collection-->
            <collection property="studentList" ofType="Student">
                <result property="id" column="sid"/>
                <result property="name" column="sname"/>
                <result property="tid" column="tid"/>
            </collection>
        </resultMap>
    
    
        <!--子查询-->
        <select id="getTeacher3" resultMap="TeacherStudent2">
            select * from teacher where id = #{tid}
        </select>
        <resultMap id="TeacherStudent2" type="Teacher">
            <collection property="studentList" column="id" javaType="ArrayList" ofType="Student" select="getStudent">
                <result property="tid" column="id"/>
            </collection>
        </resultMap>
    
        <select id="getStudent" resultType="Student">
            select * from student where tid = #{id}
        </select>
    
    
    </mapper>
    ```