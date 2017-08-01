---
title: API 指南

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up BugReport</a>
  - <a href='https://www.sunsl.net'>Documentation Powered by sunsl</a>

includes:
  - errors

search: true
---

# resservice 简介

resservice采用Restful形式提供友好接口。利用Swagger提供在线实时测试。

# 安装环境

* Linux windows Mac ARM 
* >= mongodb 2.3
* redis

# 程序目录

```shell
$:~/resservice$ tree -L 1
.
├── conf
├── dist
├── logs
├── resservice
```

conf: 目录存储配置文件

dist: 为接口文档及测试服务前端页面

logs: 程序产生日志文件

resservice: 程序文件

# 启动


> 进入程序根目录运行：

```shell
./resservice
Info [ResService] 服务启动...
Info ======================================
Info [ResService] 系统版本:0.6.6.7
Info ======================================
[restful] 2017/07/31 22:47:25 log.go:33: [restful/swagger] listing is available at http://localhost:8000/apidocs.json
[restful] 2017/07/31 22:47:25 log.go:33: [restful/swagger] http://localhost:8000/apidocs/ is mapped to folder ./dist
Info [ResService] 服务准备就绪,访问地址: http://localhost:8000

``` 

```go
```
shell进入程序目录进行启动。程序默认访问localhost的8000端口，可通过对程序所在conf目录下的app.conf进行配置。

# 配置文件

> 数据库设置

```shell
mongo.connection=localhost:27017
mongo.database=resdb
#正式环境强烈建议对mongodb进行权限设置！！！
mongo.user=   
mongo.pwd=
```


> 服务地址设置

```shell
http.server.host=localhost
http.server.port=8000
```

> 接口文档及测试地址设置

```shell
apiviews.url=localhost:8000
apiviews.home=./dist
apiviews.path=/apidocs/
```

> 存储设置

```shell
store.endpoint=localhost:9000
store.accesskey=
store.secretkey=
store.location=
```

> 接口安全校验设置

```shell
jwt.type=second
jwt.duration=60
jwt.key=111111

```

配置文件分为以下几部分：

* 数据库设置
  mongodb及redis数据配置信息
* 服务地址设置
  第三方程序可调用接口的地址
* 接口文档及测试地址设置
  接口文档呈现地址
* 存储设置
  文件存储服务配置信息
* 接口安全校验设置
  接口访问权限所需加密信息

# 接口访问
可直接通过浏览器进行访问网址进行实时测试。
接口地址可以程序启动信息中查看，默认地址为： http://localhost:8000/apidocs

<aside class="notice">
建议采用chrome或firefox最新版本进行查看
</aside>

# Login
## 登录方式有3种：
通过登录获取token进行其它接口服务操作

> 用户名登录代码:

```shell
#反馈JSON结果
curl -X GET --header 'Accept: application/json' 
'http://localhost:8000/login/account?username=$username&password=$passwd'

#反馈XML结果
curl -X GET --header 'Accept: application/xml' 
'http://localhost:8000/login/account?username=$username&password=$passwd'

```

> 邮箱登录代码:

```shell
#反馈JSON结果
curl -X GET --header 'Accept: application/json'
'http://localhost:8000/login/email?email=$eamil&password=$passwd'

#反馈XML结果
curl -X GET --header 'Accept: application/xml' 
'http://localhost:8000/login/email?email=$eamil&password=$passwd'
```

> 手机号登录代码:

```shell
#反馈JSON结果
curl -X GET --header 'Accept: application/json'
'http://localhost:8000/login/telphone?telphone=$telphone&password=$passwd'

#反馈XML结果
curl -X GET --header 'Accept: application/xml' 
'http://localhost:8000/login/telphone?telphone=$telphone&password=$passwd'
```
> 三种登录方式在成功登录后反馈以下JSON结果:

```json
{
  "Id": "string",
  "UserName": "string",
  "Role": "string",
  "Group": [
    "string"
  ],
  "Thumbnail": "string",
  "token": "string"
}
```

> 可根据需求反馈XML结果:

```xml
<?xml version="1.0"?>
<model.AuthUserInfo>
  <Id>string</Id>
  <UserName>string</UserName>
  <Role>string</Role>
  <Group>string</Group>
  <Thumbnail>string</Thumbnail>
  <token>string</token>
</model.AuthUserInfo>
```

### 通过用户名/密码登录 
<aside class="notice">
程序初始化时会产生一个默认的管理员账号：admin 密码：12345678 用于初始登录
</aside>

`GET /login/account`

### 参数

Parameter |  Description
--------- | ------- | 
username |  用户注册名称.
password |  密码.


### 通过邮箱/密码登录 

`GET /login/email`

### 参数

Parameter |  Description
--------- | ------- | 
emal |  用户注册邮箱.
password |  密码.

### 通过手机号/密码登录

`GET /login/telphone`

### 参数

Parameter |  Description
--------- | ------- | 
telphone |  用户注册手机号.
password |  密码.

<aside class="success">
必须先进行登录操作以获取到token，否则其它服务将不允许使用！！！
</aside>


# Users 

## 用户信息存储操作服务 

> users_valid

```shell
curl -X GET --header 'Accept: application/json' 
'http://localhost:8000/users/users_valid?token=1&skip=1&limit=1'
```

> users_invalid

```shell
curl -X GET --header 'Accept: application/json' 
'http://localhost:8000/users/users_invalid?token=1&skip=1&limit=1'

```

> name

```shell
curl -X GET --header 'Accept: application/json'
 'http://localhost:8000/users/name?token=1&username=1&skip=1&limit=1'
``` 

> user_id

```shell
curl -X GET --header 'Accept: text/plain' 
'http://localhost:8000/users/user_id?token=1&user_id=1'

```

> user_ids

```shell
curl -X GET --header 'Accept: application/json' 
'http://localhost:8000/users/user_ids?token=1&skip=1&limit=1&user_id=1'
```

> update

```shell
curl -X POST --header 'Content-Type: application/json' \
--header 'Accept: text/plain' -d '{ \ 
   "Id": "string", \ 
   "RealName": "string", \ 
   "UserName": "string", \ 
   "Role": "string", \ 
   "Groups": [ \ 
     "string" \ 
   ], \ 
   "Email": "string", \ 
   "Phone": "string", \ 
   "TelPhone": "string", \ 
   "Thumbnail": "string", \ 
   "Profession": "string", \ 
   "Gender": "string", \ 
   "Enable": true, \ 
   "Province": "string", \ 
   "City": "string", \ 
   "Institute": "string", \ 
   "Specialty": "string", \ 
   "Comment": "string", \ 
   "CreateTime": "2017-08-01T11:58:30.361Z", \ 
   "UpdateTime": "2017-08-01T11:58:30.361Z" \ 
 }' 'http://localhost:8000/users/update?token=1'
```

> set_user_isvalid

```shell
curl -X POST --header 'Content-Type: application/json' 
--header 'Accept: application/json' 
'http://localhost:8000/users/set_user_isvalid?token=1&user_id=1&is_valid=1'
```

> changepwd

```shell
curl -X POST --header 'Content-Type: application/json' 
--header 'Accept: text/plain' 
'http://localhost:8000/users/changepwd?token=1&user_id=1&old_pwd=1&new_pwd=1'
```

> create

```shell
curl -X PUT --header 'Content-Type: application/json' \
--header 'Accept: text/plain' -d '{ \ 
   "Id": "string", \ 
   "RealName": "string", \ 
   "UserName": "string", \ 
   "Role": "string", \ 
   "Groups": [ \ 
     "string" \ 
   ], \ 
   "Email": "string", \ 
   "Phone": "string", \ 
   "TelPhone": "string", \ 
   "Thumbnail": "string", \ 
   "Profession": "string", \ 
   "Gender": "string", \ 
   "Enable": true, \ 
   "Province": "string", \ 
   "City": "string", \ 
   "Institute": "string", \ 
   "Specialty": "string", \ 
   "Comment": "string", \ 
   "CreateTime": "2017-08-01T11:58:30.370Z", \ 
   "UpdateTime": "2017-08-01T11:58:30.370Z" \ 
 }' 'http://localhost:8000/users/create?token=1'
 ```
> delete 

```shell
curl -X DELETE --header 'Accept: application/json' 
'http://localhost:8000/users/delete?token=1&user_id=1'
```

> 单个用户反馈结构

```json
{
  "Id": "4c1b304240d29695808a3d064be7fc49",
  "RealName": "ssl",
  "UserName": "sunsl",
  "Role": "admin",
  "Groups": [],
  "Email": "admin@live.com",
  "Phone": "",
  "TelPhone": "",
  "Thumbnail": "",
  "Profession": "",
  "Gender": "",
  "Enable": true,
  "Province": "",
  "City": "",
  "Institute": "",
  "Specialty": "",
  "Comment": "",
  "CreateTime": "2017-07-28T10:14:10.749+08:00",
  "UpdateTime": "2017-07-28T10:14:10.76+08:00"
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <User>
    <Id>4c1b304240d29695808a3d064be7fc49</Id>
    <RealName>ssl</RealName>
    <UserName>sunsl</UserName>
    <PassWord>7c4a8d09ca3762af61e59520943dc26494f8941b</PassWord>
    <Role>admin</Role>
    <Email>sunsl@crtvu.edu.cn</Email>
    <Phone></Phone>
    <TelPhone></TelPhone>
    <Thumbnail></Thumbnail>
    <Profession></Profession>
    <Gender></Gender>
    <SecretKey>a3cbda8c404f3ae48031f0070ea0644b</SecretKey>
    <Enable>true</Enable>
    <Province></Province>
    <City></City>
    <Institute></Institute>
    <Specialty></Specialty>
    <Comment></Comment>
    <CreateTime>2017-07-28T10:14:10.749+08:00</CreateTime>
    <UpdateTime>2017-07-28T10:14:10.76+08:00</UpdateTime>
 </User>
```

> 用户列表反馈结构：

```json
{
  "User": [  <-用户结构数组
    {
      "Id": "4c1b304240d29695808a3d064be7fc49",
      "RealName": "ssl",
      "UserName": "sunsl",
      "Role": "admin",
      "Groups": [],
      "Email": "admin@live.com",
      "Phone": "",
      "TelPhone": "",
      "Thumbnail": "",
      "Profession": "",
      "Gender": "",
      "Enable": true, <-是否有效
      "Province": "",
      "City": "",
      "Institute": "",
      "Specialty": "",
      "Comment": "",
      "CreateTime": "2017-07-28T10:14:10.749+08:00",
      "UpdateTime": "2017-07-28T10:14:10.76+08:00"
    }
  ],
  "RecordCount": 1 #数据集中用户数量
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <Users>
    <User>
       <Id>4c1b304240d29695808a3d064be7fc49</Id>
       <RealName>ssl</RealName>
       <UserName>sunsl</UserName>
       <PassWord>7c4a8d09ca3762af61e59520943dc26494f8941b</PassWord>
       <Role>admin</Role>
       <Email>sunsl@crtvu.edu.cn</Email>
       <Phone></Phone>
       <TelPhone></TelPhone>
       <Thumbnail></Thumbnail>
       <Profession></Profession>
       <Gender></Gender>
       <SecretKey>a3cbda8c404f3ae48031f0070ea0644b</SecretKey>
       <Enable>true</Enable>
       <Province></Province>
       <City></City>
       <Institute></Institute>
       <Specialty></Specialty>
       <Comment></Comment>
       <CreateTime>2017-07-28T10:14:10.749+08:00</CreateTime>
       <UpdateTime>2017-07-28T10:14:10.76+08:00</UpdateTime>
    </User>
    <RecordCount>1</RecordCount>
 </Users>
```


###  获取所有用户信息(审核通过用户)

### HTTP Request
`GET /users/users_valid`


Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
skip  | 可为空（默认为0） |页码  | query | string
limit | 可为空 （默认为10）|每页显示行数 | query | string

###  获取所有用户信息（包含未审核通过用户）

### HTTP Request
`GET /users/users_invalid`


Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
skip  | 可为空（默认为0） |页码  | query | string
limit | 可为空 （默认为10）|每页显示行数 | query | string

### 用户名模糊查询

### HTTP Request

`GET /users/name`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
username  | 不可为空 | 用户名  | query | string
skip  | 可为空（默认为0） |页码  | query | string
limit | 可为空 （默认为10）|每页显示行数 | query | string

###  获取指定用户信息

### HTTP Request

`GET /users/user_id`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
user_id  | 不可为空 | 用户编码 | query | string

###  获取用户信息

### HTTP Request

`GET /users/user_ids`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
user_ids  | 不可为空 | 用户编码组(用户id以','隔开) | query | string
skip  | 可为空（默认为0） |页码  | query | string
limit | 可为空 （默认为10）|每页显示行数 | query | string

###  更新用户

### HTTP Request

`POST /users/update`


Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
body  | 不可为空 |结构体 | body  | model.User

###  用户有效性审核

### HTTP Request

`POST /users/set_user_isvalid`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
user_id  | 不可为空 | 用户编码 | query | string
is_valid  | 0:有效 1:无效 | 是否通过审核 | query | string

###  更新密码

### HTTP Request

`POST /users/changepwd`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
user_id  | 不可为空 | 用户编码 | query | string
old_pwd | 不可为空 | 旧密码（明文） | query | string
new_pwd  | 不可为空 | 新密码（明文） | query | string

### 创建用户,默认用户密码（111111）

### HTTP Request

`PUT /users/create`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
body  | 不可为空 |结构体 | body  | model.User

###  删除用户

### HTTP Request

`DELETE /users/delete`

Parameter|  Value | Description | ParameterType  | DataType
--------- | ------- | --------- | -------  | ------- | 
token | 不可为空 |token | query | string
user_id  | 不可为空 | 用户编码 | query | string
