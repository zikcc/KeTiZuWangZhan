> 此项目已不再继续

## 准备工作

后端：

java 17，springboot，mybatis，maven，mysql

前端：

vue3，element-ui

npm 镜像配置

```
npm config set registry https://registry.npmmirror.com
```

安装vue脚手架

```
npm install -g @vue/cli
```

环境

![image-20240215233926470](C:\Users\L\AppData\Roaming\Typora\typora-user-images\image-20240215233926470.png)

使用vue ui来创建项目

安装axios

npm install axios

设置端口号

![image-20240216001259371](C:\Users\L\AppData\Roaming\Typora\typora-user-images\image-20240216001259371.png)

仿照[欢迎访问张如范教授课题组！Welcome to Rufan Zhang Group！ (rufanzhang-group.cn)](http://www.rufanzhang-group.cn/)

## 数据库表结构

1、登录用户表users

|  id  | email | name | username | password | level_key | status | update_time | create_time |
| :--: | :---: | ---- | :------: | :------: | :-------: | ------ | :---------: | :---------: |

2、课题组成员教师表teachers

| id   | name | image | username | edu_exp | direction | introduction |
| ---- | :--: | :---: | :------: | ------- | --------- | :----------: |

3、课题组成员学生表students

|  id  | name | image | username | degree | grade | direction |
| :--: | :--: | :---: | :------: | :----: | :---: | :-------: |

4、研究成果表research

|  id  | papers | patents |
| :--: | :----: | :-----: |

## 接口文档

### 1. 登录接口

#### 1.1 基本信息

> 请求路径：/login
>
> 请求方式：post
>
> 接口描述：登录完毕后，系统下发JWT令牌。 

#### 1.2 请求参数

参数格式：json

参数说明：

| 名称     | 类型   | 是否必须 | 备注 |
| -------- | ------ | -------- | ---- |
| username | string | 必须     | 账号 |
| password | string | 必须     | 密码 |

请求样例：

```json
{
	"username": "admin",
    "password": "123456"
}
```

#### 1.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": "success",
  "data": {
      "id":1,
      "name":"管理员",
      "username":"admin",
      "token":
"eyJhbGciOiJIUzI1NiJ9.eyJlbXBJZCI6MSwiZXhwIjoxNzA4MDk3MjY3fQ.LsY9v1oQMBTc3EEzd7Qdo3QP-B6Dl3kpzxznz9lOGtA"
  }
}
```

#### 1.4 备注说明

> 用户登录成功后，系统会自动下发JWT令牌，然后在后续的每次请求中，都需要在请求头header中携带到服务端，请求头的名称为 token ，值为 登录时下发的JWT令牌。
>
> 如果检测到用户未登录，则会返回如下固定错误信息：
>
> ```json
> {
> 	"code": 0,
> 	"msg": "NOT_LOGIN",
> 	"data": null
> }
> ```

### 2. 注册接口

#### 2.1 基本信息

> 请求路径：/register
>
> 请求方式：post
>
> 接口描述：登录完毕后，系统下发JWT令牌。 

#### 2.2 请求参数

参数格式：json

参数说明：

| 名称     | 类型   | 是否必须 | 备注 |
| -------- | ------ | -------- | ---- |
| username | string | 必须     | 账号 |
| password | string | 必须     | 密码 |
| email    | string | 必须     | 邮箱 |
| name     | string | 必须     | 名称 |

请求样例：

```json
{
	"username": "admin",
    "password": "123456",
    "email": "xxxx@xxx.com",
    "name":"xxx"
}
```

#### 2.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 非必须   |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": null
}
```

### 3. 获取成员接口

#### 3.1 基本信息

> 请求路径：/members
>
> 请求方式：get
>
> 接口描述：获取教师和学生信息 

#### 3.2 请求参数

无

#### 3.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": {
    "member_info": {
        "teachers": [
            {
                "name": "xxx",
                "image": "http://xxx.com",
                "eduExp": "xxx",
                "direction": "xxx"
            }
        ],
        "students": [
            {
                "name": "xxx",
                "image": "http://xxx.com",
                "degree": "xx",
                "grade": "xxxx",
                "direction": "xxx"
            },
            {
                "name": "xxx",
                "image": "http://xxx.com",
                "degree": "xx",
                "grade": "xxxx",
                "direction": "xxx"
            }
        ]
    }
}
```

#### 3.4 备注说明

> java里封装两个表的实体类，然后把实体类装入到 Map<String, List<?>> 中，一起返回数据。
>
> 后续可以优化

### 4. 获取研究成果接口

#### 4.1 基本信息

> 请求路径：/works
>
> 请求方式：get
>
> 接口描述：获取研究成果 

#### 4.2 请求参数

无

#### 4.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": {
    "works": [
        {
            "id": 1,
            "papers": "论文1",
            "patents": "专利1" 
        },
        {
            "id": 2,
            "papers": "论文2",
            "patents": "专利2"
        }
    ]
  }
}
```

### 5. 获取教师信息接口

#### 5.1 基本信息

> 请求路径：/t_info
>
> 请求方式：get
>
> 接口描述：获取教师信息

#### 5.2 请求参数

无

#### 5.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": [
    {
        "id": 1,
        "name": "xxx",
        "image": "http://xxx.com",
        "username": "niyuanzhi",
        "eduExp": "xxx",
        "direction": "xx",
        "introduction": "xxxxxx"
    }
  ]
}
```

### 6. 根据账号获取教师信息接口

#### 6.1 基本信息

> 请求路径：/teac
>
> 请求方式：get
>
> 接口描述：获取教师信息

#### 6.2 请求参数

| 名称     | 类型   | 是否必须 | 默认值 | 备注 | 其他信息 |
| -------- | ------ | -------- | ------ | ---- | -------- |
| username | string | 必须     |        | 账号 |          |

#### 6.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": {
    "id": 1,
    "name": "xxx",
    "image": "http://xxx.com",
    "username": "niyuanzhi",
    "eduExp": "xxx",
    "direction": "xx",
    "introduction": "xxxxxx"
  }
}
```

### 7. 修改教师信息接口

#### 7.1 基本信息

> 请求路径：/teac
>
> 请求方式：put
>
> 接口描述：修改教师信息 

#### 7.2 请求参数

参数格式：json

参数说明：

| 名称         | 类型   | 是否必须 | 默认值 | 备注     |
| ------------ | ------ | -------- | ------ | -------- |
| id           | int    | 必须     |        | id       |
| name         | string | 必须     |        | 姓名     |
| image        | string | 非必须   |        | 图片     |
| edu_exp      | string | 必须     |        | 教育经历 |
| direction    | string | 必须     |        | 研究方向 |
| introduction | string | 非必须   |        | 介绍     |

#### 7.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data":null
}
```

### 8. 根据账号获取学生信息接口

#### 8.1 基本信息

> 请求路径：/stud
>
> 请求方式：get
>
> 接口描述：获取学生信息

#### 8.2 请求参数

| 名称     | 类型   | 是否必须 | 默认值 | 备注 | 其他信息 |
| -------- | ------ | -------- | ------ | ---- | -------- |
| username | string | 必须     |        | 账号 |          |

#### 8.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data": {
    "id": 1,
    "name": "xxx",
    "image": "http://xxx.com",
    "username": "xxx",
    "degree": "xx",
    "grade": "xxxx",
    "direction": "xx"
  }
}
```

### 

### 9. 修改学生信息接口

#### 9.1 基本信息

> 请求路径：/stud 
>
> 请求方式：put
>
> 接口描述：修改学生信息 

#### 9.2 请求参数

参数格式：json

参数说明：

| 名称      | 类型   | 是否必须 | 默认值 | 备注     |
| --------- | ------ | -------- | ------ | -------- |
| name      | string | 必须     |        | 姓名     |
| image     | string | 非必须   |        | 图片     |
| degree    | string | 必须     |        | 教育经历 |
| grade     | string | 必须     |        | 研究方向 |
| direction | string | 非必须   |        | 介绍     |

#### 9.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": null,
  "data":null
}
```

### 10.校验token接口

#### 10.1 基本信息

> 请求路径：/token
>
> 请求方式：post
>
> 接口描述：校验token是否合法。 

#### 10.2 请求参数

| 名称  | 类型   | 是否必须 | 默认值 | 备注 | 其他信息 |
| ----- | ------ | -------- | ------ | ---- | -------- |
| token | string | 必须     |        |      |          |

#### 10.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": "success",
  "data": {
      "id":1,
      "name":"管理员",
      "username":"admin"
  }
}
```

### 11.退出登录接口

#### 11.1 基本信息

> 请求路径：/logout
>
> 请求方式：post
>
> 接口描述：退出登录。 

#### 11.2 请求参数

| 名称  | 类型   | 是否必须 | 默认值 | 备注 | 其他信息 |
| ----- | ------ | -------- | ------ | ---- | -------- |
| token | string | 必须     |        |      |          |

#### 11.3 响应数据

参数格式：json

参数说明：

| 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------ | -------- |
| code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
| msg  | string | 非必须   |        | 提示信息                 |          |
| data | string | 必须     |        | 返回的数据               |          |

响应数据样例：

```json
{
  "code": 1,
  "msg": "success",
  "data": {}
}
```







## springboot开发记录

pojo:  UserLoginDTO------UserLoginVo------User

server: UserController------UserMapper------UserService  UserServiceImpl

## Vue3开发记录

```html
<router-link to="/login">登录</router-link>|
<router-link to="/register">注册</router-link>|
<router-link to="/stud">学生端</router-link>|
<router-link to="/teac">教师端</router-link>|


<router-link to="/">首页</router-link> |
<router-link to="/members">课题组成员</router-link>|
<router-link to="/t_info">教师信息</router-link>|
<router-link to="/works">研究成果</router-link>
       
```

