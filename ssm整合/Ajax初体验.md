# Ajax初体验

#### 1、在百度搜索JQuery下载jquery.js

#### 2、在spring-servlet.xml中配置静态资源过滤

```xml
 <!--静态资源过滤-->
    <mvc:default-servlet-handler/>
```

#### 3、浏览器头引入jquery.js

```html
<script src="${pageContext.request.contextPath}/static/js/jquery.js"></script>
```

#### 4、编写ajax的相关信息,在页面添加一个input标签，然后实现

```java
//失去焦点的时候  
<input type="text" id="username" onblur="a()">
      
      
<script src="${pageContext.request.contextPath}/static/js/jquery-3.4.1.js"></script>

  <script>
    function a(){
      $.post({
        url:"${pageContext.request.contextPath}/a1",
        data:{"name":$("#username").val()},
        success:function (data) {
          alert(data)
        }
      })
    }

  </script>
```

## 传递JSON，List对象的时候出错

#### 1、现在pom.xml文件里面添加相对应的依赖包,之后在lib包添加相对应的包

```xml
<dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.5.4</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.5.4</version>
        </dependency>
```

#### 2、在spring-servlet.xml文件添加相对应的

```xml
<mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter"/>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>
```

#### 3、JSON乱码问题，在spring-servlet.xml文件添加相对应的，可覆盖上面的配置

```xml
<mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
```

#### 4、实现效果

![image-20201029193923623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201029193923623.png)

```javascript
 <script>
        function a1() {
            $.post({
                url:"${pageContext.request.contextPath}/a3",
                data:{"name":$("#name").val()},
                success:function (data) {
                    console.log(data)
                    if(data === "ok"){
                        $("#userInfo").css("color","green");
                    }else{
                        $("#userInfo").css("color","red");
                    }
                    $("#userInfo").html(data)
                }
            })
        }
        function a2() {
            $.post({
                url:"${pageContext.request.contextPath}/a3",
                data:{"pwd":$("#pwd").val()},
                success:function (data) {
                    console.log(data)
                    if(data.toString() === 'ok'){
                        $("#pwdInfo").css("color","green");
                    }else{
                        $("#pwdInfo").css("color","red");
                    }
                    $("#pwdInfo").html(data)
                }
            })
        }
    </script>
```

