## 创建版本库

### 1、在本地创建一个SvnRep的包当做本地版本库

### 2、

![image-20201126161225080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201126161225080.png)

3、进入OA，创建版本库

```java
svnadmin create E:\WorkPlaceTotal\SvnRep\OA
```

4、启动本地的Svn服务器

启动的时候不能直接指定OA，否则会报错

```java
svnserve -d -r E:\WorkPlaceTotal\SvnRep
```

5、命令行

6、使用方式

可以在某个文件夹右键，选择Svn Checkout 可以把仓库的东西下载下来，要先连接上仓库才可以显示update和commit，commit是用来提交我们修改的文件，update的话是更新Svn仓库的数据到我们本地仓库



