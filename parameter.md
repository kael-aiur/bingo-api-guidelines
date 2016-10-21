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