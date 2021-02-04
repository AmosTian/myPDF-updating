# IDEA

@(IDEA)[2021年2月4日08:50:05]

[TOC]

<div style="page-break-after:always"></div>

## IDEA目录结构

![image-20210204085247994](IDEA.assets/image-20210204085247994.png)![image-20210204085318627](IDEA.assets/image-20210204085318627.png)

```xml
-Xms128m， 16 G 内存的机器可尝试设置为 -Xms512m
(设置初始的内存数，增加该值可以提高 Java 程序的启动速度。 )
-Xmx750m， 16 G 内存的机器可尝试设置为 -Xmx1500m
(设置最大内存数，提高该值，可以减少内存 Garage 收集的频率，提高程序性能)
-XX:ReservedCodeCacheSize=240m， 16G 内存的机器可尝试设置为
-XX:ReservedCodeCacheSize=500m
(保留代码占用的内存容量)
```

### 设置的目录结构

![image-20210204085425584](IDEA.assets/image-20210204085425584.png)![image-20210204085438283](IDEA.assets/image-20210204085438283.png)

<div style="page-break-after:always"></div>

## IDEA工程

### 设置视图

![image-20210204085631450](IDEA.assets/image-20210204085631450.png)

### 工程界面

![image-20210204085715899](IDEA.assets/image-20210204085715899.png)

-   src：存放源码
-   .idea 和 [项目名].iml：IDEA工程特有

### 模块

![image-20210204090130370](IDEA.assets/image-20210204090130370.png)![image-20210204090325423](IDEA.assets/image-20210204090325423.png)

#### IDEA中创建Module

![image-20210204090448588](IDEA.assets/image-20210204090448588.png)![image-20210204090502145](IDEA.assets/image-20210204090502145.png)

#### 删除Module

1.  将Module与Project解除关联

![image-20210204090657798](IDEA.assets/image-20210204090657798.png)

2.  删除Module

![image-20210204090748373](IDEA.assets/image-20210204090748373.png)

### 查看项目配置

![image-20210204090943967](IDEA.assets/image-20210204090943967.png)

<div style="page-break-after:always"></div>

## 编辑器配置

设置界面

![image-20210204091041327](IDEA.assets/image-20210204091041327.png)

### Editor——General

#### 设置自动导包功能

![image-20210204093627834](IDEA.assets/image-20210204093627834.png)

#### 设置鼠标滚轮修改字体大小

![image-20210204093800052](IDEA.assets/image-20210204093800052.png)

#### 显示行号和方法间的分割线

![image-20210204093732240](IDEA.assets/image-20210204093732240.png)

#### 忽略大小写提示

![image-20210204094154036](IDEA.assets/image-20210204094154036.png)

-   stringBuffe  & StringBuffer  

#### 设置单行显示tabs

![image-20210204094319046](IDEA.assets/image-20210204094319046.png)

### Editor——Color Scheme

#### 修改代码中注释的字体颜色

![image-20210204094500466](IDEA.assets/image-20210204094500466.png)

-   Doc Comment-Text：文档注释
-   Block Comment：多行注释
-   Line Comment：单行注释

### Editor——Code Style

![image-20210204094755748](IDEA.assets/image-20210204094755748.png)

-   设置超过指定 import 个数，改为*   

### Editor——File and Code Templates

#### 新建文档的头部注释信息

![image-20210204094906387](IDEA.assets/image-20210204094906387.png)

### Editor——File Encodings

#### 设置项目文件编码

![image-20210204095105213](IDEA.assets/image-20210204095105213.png)

-   Transparent native-to-ascii conversion 主要用于转换 ascii，一般都要勾选，
    不然 Properties 文件中的注释显示的都不会是中文  

##### 对单一文件的编码修改

 ![image-20210204095438136](IDEA.assets/image-20210204095438136.png)

<div style="page-break-after:always"></div>

## 编译配置——Build,Execution,Deployment

### Compiler——设置自动编译

![image-20210204101446563](IDEA.assets/image-20210204101446563.png)

### 断点调试

![image-20210204125156474](IDEA.assets/image-20210204125156474.png)

![image-20210204125216603](IDEA.assets/image-20210204125216603.png)![image-20210204125235440](IDEA.assets/image-20210204125235440.png)

### 窗口划分方式

![image-20210204101621839](IDEA.assets/image-20210204101621839.png)

<div style="page-break-after:always"></div>

## Keymap

![image-20210204101815521](IDEA.assets/image-20210204101815521.png)![image-20210204101834301](IDEA.assets/image-20210204101834301.png)![image-20210204101850736](IDEA.assets/image-20210204101850736.png)

![image-20210204101930584](IDEA.assets/image-20210204101930584.png)![image-20210204101942658](IDEA.assets/image-20210204101942658.png)

### 快捷键

![image-20210204102005462](IDEA.assets/image-20210204102005462.png)![image-20210204102014170](IDEA.assets/image-20210204102014170.png)

<div style="page-break-after:always"></div>

## 模板

### 1.Live Templates(实时代码模板)功能介绍  

配置一些常用代码字母缩写，在输入简写时可以出现你预定义的固定模式的代码，使得开发效率大大提高， 同时也可以增加个性化。最简单的例子就是在 Java 中输入 sout 会出现 System.out.println();  

### 举例

![image-20210204110430131](IDEA.assets/image-20210204110430131.png)![image-20210204110442390](IDEA.assets/image-20210204110442390.png)![image-20210204110509540](IDEA.assets/image-20210204110509540.png)![image-20210204110800773](IDEA.assets/image-20210204110800773.png)![image-20210204110841484](IDEA.assets/image-20210204110841484.png)

<div style="page-break-after:always"></div>

## 静态JavaWeb和Tomcat

![image-20210204111809724](IDEA.assets/image-20210204111809724.png)![image-20210204111824133](IDEA.assets/image-20210204111824133.png)![image-20210204111843762](IDEA.assets/image-20210204111843762.png)![image-20210204111901538](IDEA.assets/image-20210204111901538.png)![image-20210204111915919](IDEA.assets/image-20210204111915919.png)![image-20210204111925739](IDEA.assets/image-20210204111925739.png)![image-20210204111937227](IDEA.assets/image-20210204111937227.png)

<div style="page-break-after:always"></div>

## 关联数据库

### 启动Mysql服务

![image-20210204112333857](IDEA.assets/image-20210204112333857.png)

![image-20210204112756557](IDEA.assets/image-20210204112756557.png)

### 设置时区

![image-20210204112915382](IDEA.assets/image-20210204112915382.png)

>   配置mysql时区

```mysql
show variables like'%time_zone';
```

![image-20210204113510601](IDEA.assets/image-20210204113510601.png)

```mysql
set global time_zone = '+8:00';
```

![image-20210204113621201](IDEA.assets/image-20210204113621201.png)

![image-20210204113228370](IDEA.assets/image-20210204113228370.png)

### 设置自动同步

![image-20210204113829805](IDEA.assets/image-20210204113829805.png)

>    配置好了 Database 之后， IntelliJ IDEA 会自动识别 domain对象与数据表的关系，也可以通过 Database 的数据表直接生成 domain 对象  

![image-20210204114104271](IDEA.assets/image-20210204114104271.png)

![image-20210204114251214](IDEA.assets/image-20210204114251214.png)

<div style="page-break-after:always"></div>

## IDEA中使用git

### 配置

![image-20210204115553364](IDEA.assets/image-20210204115553364.png)

>   IntelliJ IDEA 是自带对这些版本控制工具的插件支持，但是该装什么版本控制客户端还是要照样装的
>
>   IntelliJ IDEA 对版本控制的支持是以插件化的方式来实现的

![image-20210204115809389](IDEA.assets/image-20210204115809389.png)![image-20210204115900240](IDEA.assets/image-20210204115900240.png)![image-20210204120052382](IDEA.assets/image-20210204120052382.png)

5.  从远程仓库获取项目

    ![image-20210204121552322](IDEA.assets/image-20210204121552322.png)![image-20210204121401217](IDEA.assets/image-20210204121401217.png)
    
    6.  新建git本地仓库
    
        ![image-20210204190033090](IDEA.assets/image-20210204190033090.png)
    
        ![image-20210204190122404](IDEA.assets/image-20210204190122404.png)
    
        7.  添加忽略同步文件
    
            ![image-20210204190436235](IDEA.assets/image-20210204190436235.png)
    
            ![image-20210204190544136](IDEA.assets/image-20210204190544136.png)

![image-20210204120232236](IDEA.assets/image-20210204120232236.png)![image-20210204120246775](IDEA.assets/image-20210204120246775.png)

![image-20210204120306186](IDEA.assets/image-20210204120306186.png)![image-20210204120315312](IDEA.assets/image-20210204120315312.png)

### IDEA中的GIT基本操作

#### 版本控制

##### 查看历史版本——git log

![image-20210204191130478](IDEA.assets/image-20210204191130478.png)

##### 版本切换

1.  获取目标hash

    ![image-20210204191326333](IDEA.assets/image-20210204191326333.png)

2.  切换HEAD指针

    ![image-20210204191353092](IDEA.assets/image-20210204191353092.png)

>   选择git reset方式

![image-20210204191450239](IDEA.assets/image-20210204191450239.png)

>   结果

![image-20210204191522653](IDEA.assets/image-20210204191522653.png)

![image-20210204191831322](IDEA.assets/image-20210204191831322.png)

#### 分支管理

##### 新建分支

![image-20210204205204346](IDEA.assets/image-20210204205204346.png)

![image-20210204205243576](IDEA.assets/image-20210204205243576.png)

##### 切换分支

![image-20210204205327601](IDEA.assets/image-20210204205327601.png)

##### 合并分支

![image-20210204205851724](IDEA.assets/image-20210204205851724.png)

![image-20210204205908120](IDEA.assets/image-20210204205908120.png)

##### 冲突处理

![image-20210204210023447](IDEA.assets/image-20210204210023447.png)

-   Accept Yours：master分支

-   Accept Theirs：dev分支

-   Merge：手动合并

    ![image-20210204210322509](IDEA.assets/image-20210204210322509.png)

    >    master分支

    ![image-20210204210541289](IDEA.assets/image-20210204210541289.png)

    >    dev分支：保持合并到master的状态

    ![image-20210204210626072](IDEA.assets/image-20210204210626072.png)

##### push到远程库

![image-20210204212425760](IDEA.assets/image-20210204212425760.png)

![image-20210204213205864](IDEA.assets/image-20210204213205864.png)

![image-20210204213313481](IDEA.assets/image-20210204213313481.png)

#### 从远程库clone

![image-20210204211254170](IDEA.assets/image-20210204211254170.png)

![image-20210204211313767](IDEA.assets/image-20210204211313767.png)

<div style="page-break-after:always"></div>

## 关闭自动更新

![image-20210204122002978](IDEA.assets/image-20210204122002978.png)

<div style="page-break-after:always"></div>

## 清空所有缓存和索引

IntelliJ IDEA 首次加载项目的时候，都会创建索引 ，创建索引的时间跟项目的文件多少成正比

IntelliJ IDEA 的缓存和索引主要是用来加快文件查询，从而加快各种查找、代码提示等操作的速度

![image-20210204122325868](IDEA.assets/image-20210204122325868.png)

-   清除索引和缓存会使得 IntelliJ IDEA 的 Local History 丢失。所以如果你项目没有加入到版本控制，而你又需要你项目文件的历史更改记录，那你最好备份下你的LocalHistory 目 录。

    >    目录地址：C:\Users\Auspice Tian\AppData\Local\JetBrains\IntelliJIdea2020.1\LocalHistory

![image-20210204124449573](IDEA.assets/image-20210204124449573.png)