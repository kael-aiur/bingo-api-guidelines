# 请求方法规范

在restful的建议标准中，对不同的操作规定了使用不同的请求方法，品高api网关也有类似的规定，但是简化了一些规范，以便更好的理解和使用。

|请求方法|操作|示例|
|----|----|----|
|POST|创建数据|POST /student|
|PATCH|修改数据|PATCH /student/{id}|
|DELETE|删除数据|DELETE /student/{id}|
|GET|查询数据|GET /student?filters=name eq Tom|
|POST|业务操作|POST /status/student?status=disable&studentId={studentId}|

## 创建数据

创建数据指的是要创建一个模型的记录，我们规定使用`POST`方法，如：

```
POST /school :创建一个学校的记录
POST /classes :创建一个班级的记录
POST /student :创建一个学生的记录
POST /score :创建一个分数的记录
```

## 修改数据

修改模型数据，使用`PATCH`请求，如下：

```
PATCH /school/{schoolId}   :修改一个学校的记录
PATCH /classes/{classesId} :修改一个班级的记录
PATCH /student/{studentId} :修改一个学生的记录
PATCH /score/{scoreId}     :修改一个分数的记录
```

## 删除数据

删除数据，使用`DELETE`请求，如：

```
DELETE /school/{schoolId}   :删除一个学校的记录
DELETE /classes/{classesId} :删除一个班级的记录
DELETE /student/{studentId} :删除一个学生的记录
DELETE /score/{scoreId}     :删除一个分数的记录

```

## 查询记录

查询记录使用`GET`请求，如：

```
GET /student/{studentId}                                   :查询单个学生
GET /student                                               :查询学生
GET /statistics/average?classesId={classesId}&subject=math :查询班级数学成绩平均分
```

## 业务操作

对于会改变数据状态的业务操作，使用'POST'方法，比如:

```
POST /status/student?status=disable&studentId={studentId} :设置学生信息失效
```
