

## GitHub

### 第一种方式

### 1、把自己的项目放在github托管

复制仓库的地址

![image-20201201091259782](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201201091259782.png)

### 2、在自己项目的上一级底下用cmd，把GitHub的内容克隆下来

此时我们的项目下会有一个supermall的文件

```java
git clone https://github.com/linguiquan923/supermall.git
```

### 3、把项目代码复制到supermall下，

![image-20201201091927640](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201201091927640.png)

这两个是不需要的

![image-20201201091947670](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201201091947670.png)



### 4、把代码上传到github

进入supermall里面

查看文件状态，new files 为未托管

```java
git status
```

把文件添加到本地仓库中

```java
git add .
```

提交到本地仓库中

```java
git commit -m '初始化项目'
```

提交到github

```java
git push
```

以后更新代码后只需要（），就可以提交代码了

```java
git add .
git commit -m '初始化项目'
git push
```

### 第二种方式（常用）

#### 使用 git remote -v 可以查看远程仓库分支

新建一个空仓库后里面会有代码，只需要在想上传的仓库执行语句就可以

#### 1、实现把本地的代码上传到仓库中，这时候仓库是新的

这些语句新建仓库的时候会有，要在要从上传的文件目录下执行语句

```java
//初始化一个文件夹，让它有.git文件
git init  
//加入本地仓库
git add .
//提交到本地仓库
git commit -m 'message'

//加入到origin
git remote add origin
https://github.com/linguiquan923/testmall.git
//选择分支
git branch -M main/master
//上传
git push -u origin main/master
```

#### 2、更新代码带GitHub中

```java
git add .
```

提交到本地仓库

```java
git commit -m 'message'
```

上传到github，需要账号密码

```java
git push
```

#### 3、从GitHub更新代码到本地仓库

###### 1、查看远程仓库分支

```java
git remote -v
```

###### 2、从远程仓库获取文件，要指定分支

```java
git fetch origin main:temp
```

###### 3、比较不同

```java
git diff temp
```

###### 4、合并文件，合并temp和本地的文件

```java
git merge temp
```

