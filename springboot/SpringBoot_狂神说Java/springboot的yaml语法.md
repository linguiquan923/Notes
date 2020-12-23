# springboot的yaml语法

```yaml
#对空格的要求十分高
#对象
student:
  name: lgq
  age: 23
  
#行内写法
student1: {name: lgq,age: 23}

#数组
pets:
  - cat
  - dog
  - pig
    
#行内写法
pets1: [cat,dog,pig]
```



#### yaml可以直接给实体类赋值

#### 1、先创建好实体类

```
@Value("旺财")
private String name;
@Value("3")
private int age;
```

#### 2、yaml注入值

在配置文件里编写

```yaml
person:
  name: 林贵全
  age: 23
  happy: true
  birth: 1998/11/11
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
  dog:
    name: 旺财
    age: 3
```

在需要注入值得类前添加

**@ConfigurationProperties(prefix = "person")**

#### 	数据校验:强制输入为email格式

```yaml
@Component
@ConfigurationProperties(prefix = "person")
@Validated//数据校验
public class Person {
    @Email
    private String name;
```

#### 多文件配置优先级：

./config/

./

classpath:/config/

classpath:/

指定配置文件，多文档模块

```yaml
#springboot多环境配置，可以选择激活哪一个配置温家
spring:
  profiles:
    active: test
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: test
```



