项目编码命名规范
===

## 总体原则

包名所有字母小定，类名采用“驼峰式”命名。

所有的包下必须包含一个package-info.java文件，介绍该包的用途。

## 实体层类

包命名规范:  
    
    com.xxx.微服务名.domain  
    
类命名规范：同数据库表名，使用单数。   
如:  

    User

# 持久层接口

由于在MyBatis中一般只需要接口便可以解决所有的数据库访问工作，所以数据库操作都放在一个包下。  
包命名规范：  

    com.xxx.微服务名.persistence
    
如权限管理子系统：

    com.xxx.user.persistence
    
接口命名规范：

    实体名 + Repository
    
如:

    UserRepository
    
MyBatis配置文件存放于持久层接口同文件夹下。   
文件命名同接口文件名，使用.xml做为文件扩展名。   
如： 

    UserRepository.xml
    
    
## 服务层接口

包命名规范:

    com.xxx.微服务名.service
    
接口命名规范：

    实体名 + Service

如:

    UserService
    
## 服务层实现类

包命名规范:

    com.xxx.微服务名.service.impl
    
类命名规范：

    实体名 + ServiceImpl
    
如:

    UserServiceImpl
    
## 控制器层

包命名规范：

    com.xxx.微服务名.controller
    
类命名规范：

    实体名 + Controller
    
如:

    UserController
    
## 工具层公共类

命名规范：

    com.xxx.微服务名.util
    
类命名规范：

    功能 + Util
    
如:

    StringUtil
    
注：如可用性较强，可将其抽取到对应的全局工具工程中。

## Spring配置文件

除了SpringBoot启动时可选的application.yml(.properties)和bootstrap.yml(.properties)外，其他的强制零配置。

## 其他命名规范

### 变量命名

使用小驼峰式命名法。   
变量名首字母必须小写。   
有多个单词组成时，后面的单词首字母大写，单词与单词之间不要使用"_"做连接。   
变量名访问控制必须为私有， 可以对其增加setter与getter方法。   

### 常量命名

所有字母大写。   
如果有多个单词组成，单词与单词之间以"_"隔开。   

### 方法命名

使用小驼峰式命名法。   
变量名首字母必须小写。   
有多个单词组成时，后面的单词首字母大写，单词与单词之间不要使用"_"做连接。   