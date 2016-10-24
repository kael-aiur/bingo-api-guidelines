# api参数规范

|api作用|参数位置|示例|说明|
|----|----|----|----|
|创建数据|body|POST /student body:`{name:"Tom",sex:'1'}`|建议使用json格式的请求体|
|修改数据|body|POST /student/{id} body:`{name:Lucy}`|建议使用json格式的请求体|
|查询数据|queryString/path|GET /student/{id}||
|删除数据|path/queryString|DELETE /student/{id} 或 DELETE /student?id={id}|单行删除(根据id)建议使用path传递参数，根据指定条件删除多行记录建议使用queryString|
|需要写数据的业务操作|body|POST /status/disable body:`studentId={id}`|建议使用form-urlencoded的请求体传递参数|
|文件上传一类的操作|body||建议使用form-data的请求体传递参数和文件流|

## url参数传递

url参数传递，是指将参数值作为url的一部分，用户访问url的时候，服务端从url中解析出参数值，如：

```
GET /student/{id}                    :查询学生信息
GET /statistic/{classesId}/average   :查询班级平均分
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
GET /statistic/average?classesId=7h72GggUMsn&subject=math :查询班级数学平均分
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

上面的是http的请求报文，我们在请求头中指定了请求体类型是json，因此api解析只需要将请求体读出来，并以json的方式解析即可。

### form-data请求体

form-data的请求体，需要添加请求头`Content-Type: multipart/form-data`，同样以创建学生为例，请求报文如下：

```
POST /student HTTP/1.1
Host: localhost:8080
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Cache-Control: no-cache
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="name"

Lucy
------WebKitFormBoundary7MA4YWxkTrZu0gW

Content-Disposition: form-data; name="classesId"

7h72GggUMsn
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="schoolId"

7hUdfgwURaH
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

这个请求报文，多数情况下web应用容器都能自动解析出参数，如果我们开发的api不是通过web容器部署的话，可以自行选择http请求解析的工作进行解析。

### form-urlencoded请求体

form-urlencoded请求体，一般是将参数以key-value的方式组织，并以url编码的方式编码特殊字符，请求体格式和queryString相似，需要在请求头添加`Content-Type: application/x-www-form-urlencoded`,举例如下：

```
POST /student HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

name=Lucy&classesId=7h72GggUMsn&schoolId=7hUdfgwURaH
```

这里的请求体就是`name=Lucy&classesId=7h72GggUMsn&schoolId=7hUdfgwURaH`，一般情况下web容器都能自动解析这个请求体。
