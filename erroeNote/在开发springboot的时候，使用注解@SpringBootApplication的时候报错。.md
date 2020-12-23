	在开发springboot的时候，使用注解@SpringBootApplication的时候报错。

##### 	第一步：检查自己maven安装目录（E:\java\springboot\apache-maven-3.3.9\conf）下的setting.xml文件是否配置好，需要添加以下配置

　　

 <profiles> 

　　<id>jdk-1.8</id>  

 　　 <activation>    

​    　　 <activeByDefault>true</activeByDefault>  

​    　　 <jdk>1.8</jdk>   

　　   </activation> 

   <properties> 

​     <maven.compiler.source>1.8</maven.compiler.source> 

​     <maven.compiler.target>1.8</maven.compiler.target> 

​     <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  

   </properties>

 </profiles>



 <repositories>

  <repository>

   <id>alimaven</id>

   <name>aliyun maven</name>

   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>

  </repository>



  <repository>

   <id>spring-snapshots</id>

   <url>http://repo.spring.io/libs-snapshot</url>

  </repository>

</repositories>



<pluginRepositories>

  <pluginRepository>

   <id>spring-snapshots</id>

   <url>http://repo.spring.io/libs-snapshot</url>

  </pluginRepository>

</pluginRepositories>

​	

##### 	第二步：检查自己file->setting->maven中的maven是否有配置好



##### 	第三步：检查External Liberals文件夹下面是否已经导入MAVEN依赖包

