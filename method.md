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

创建记录的时候，模型相应的属性通过参数传递，详细的参数传递方式在[api参数规范](parameter.md)中定义。

## 修改数据

修改模型数据，使用`PATCH`请求，如下：

```
PATCH /school/{schoolId}   :修改一个学校的记录
PATCH /classes/{classesId} :修改一个班级的记录
PATCH /student/{studentId} :修改一个学生的记录
PATCH /score/{scoreId}     :修改一个分数的记录
```

修改记录的时候，需要修改记录的哪些属性，需要通过参数指定，参数的传递方式[api参数规范](parameter.md)中定义。

# 删除数据

删除数据，使用`DELETE`请求，我们同样只规定对单行记录的删除，对于多行记录的删除，同样是建议在具体的业务模块api中定义，但是一样使用`POST`请求。单行记录的删除示例如下：

```
DELETE /school/{schoolId}   :删除一个学校的记录
DELETE /classes/{classesId} :删除一个班级的记录
DELETE /student/{studentId} :删除一个学生的记录
DELETE /score/{scoreId}     :删除一个分数的记录

```



删除记录的情况下，一般不需要特殊的参数，但是有些时候，由于关系存在，为了保证数据完整性，需要通过参数指定删除时对关系的处理，这个参数规范将在[api参数规范](parameter.md)中定义。




## 业务操作

对于会改变数据状态的业务操作，使用'POST'方法，比如:

```
POST /status/student?status=disable&studentId={studentId} :设置学生信息失效
```
