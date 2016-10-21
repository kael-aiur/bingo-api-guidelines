# api参数规范

网关的api参数一般有三种方式传递：

|位置|说明|示例|
|----|----|----|
|path|通过url传递|/student/{id}|
|queryString|通过查询字符串传递|/student?id={id}|
|body|通过请求体传递||

## url参数传递

url参数传递，是指将参数值作为url的一部分，用户访问url的时候，服务端从url中解析出参数值，如：

```
查询学生信息：GET /student/{id}
查询班级平均分：GET /statistic/{classesId}/average
```

如果访问如下路径：

```
GET /student/7D82fxxw1jn
GET /statistic/7h72GggUMsn/average
```

api应该能够解析得到参数id=7D82fxxw1jn和classesId=7h72GggUMsn

## 查询字符串参数传递

查询字符串的参数传递其实就是标准的http查询参数传递，即在url后以`?`标识参数段，参数以`key=value`的方式传递，多个参数之间通过`&`分隔，例如：

```
查询班级数学平均分：GET /statistic/average?classesId=7h72GggUMsn&subject=math
```

## 请求体参数传递

通过请求体传递参数有多种方式，按照http协议的标准规范，一般需要通过请求头指定请求体类型，然后根据请求体类型解析请求体获取参数。

我们以三种常用的请求体格式来举例：

```
json：请求体是一个json字符串
form-data:请求体是一个二进制串
form-urlencoded:请求体是url编码的key-value对
```

### json请求体

请求体是json的情况下，一般需要添加请求头`Content-Type: application/json`，比如我们需要创建一个学生的时候，通过如下请求：

```
POST /student HTTP/1.1
Host: localhost:8080
Content-Type: application/json
Cache-Control: no-cache

{
    name:"Lucy",
    classesId:"7h72GggUMsn",
    schoolId:"7hUdfgwURaH"
}
```