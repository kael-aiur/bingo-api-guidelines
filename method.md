# 请求方法规范

在restful的建议标准中，对不同的操作规定了使用不同的请求方法，品高api网关也有类似的规定，但是简化了一些规范，以便更好的理解和使用。

|请求方法|操作|示例|
|----|----|----|
|POST|创建数据|POST /student|
|PATCH|修改数据|PATCH /student/{id}|
|DELETE|删除数据|DELETE /student/{id}|
|GET|查询数据|GET /student?filters=name eq Tom|
|POST|需要写数据的业务操作|POST /status/student?status=disable&studentId={studentId}|

---

## 修改数据

修改模型数据，使用`PATCH`请求，我们只规定对单行记录的修改，对于多行记录的修改，一般情况下不建议在模型的api下开放多行修改的接口，有某些特定的业务需求需要的话，多行修改应该在业务模块中，使用`POST`请求。对于单行记录的修改，请求如下：

```
修改一个学校的记录：PATCH /school/{schoolId}
修改一个班级的记录：PATCH /classes/{classesId}
修改一个学生的记录：PATCH /student/{studentId}
修改一个分数的记录：PATCH /score/{scoreId}
```

修改记录的时候，需要修改记录的哪些属性，需要通过参数指定，参数的传递方式[api参数规范](parameter.md)中定义。

---

## 删除数据

删除数据，使用`DELETE`请求，我们同样只规定对单行记录的删除，对于多行记录的删除，同样是建议在具体的业务模块api中定义，但是一样使用`POST`请求。单行记录的删除示例如下：

```
删除一个学校的记录：DELETE /school/{schoolId}
删除一个班级的记录：DELETE /classes/{classesId}
删除一个学生的记录：DELETE /student/{studentId}
删除一个分数的记录：DELETE /score/{scoreId}
```

删除记录的情况下，一般不需要特殊的参数，但是有些时候，由于关系存在，为了保证数据完整性，需要通过参数指定删除时对关系的处理，这个参数规范将在[api参数规范](parameter.md)中定义。

---

## 查询记录

查询记录使用`GET`请求，查询结果可以是单行或者多行，可以支持id查询和条件查询，id查询的url示例如下：

```
查询单个学生：GET /student/{studentId}
```

如果不使用id查询，示例如下：

```
查询学生: GET /student
```

默认直接发这个请求会查询所有学生记录，但是可以通过参数指定过滤条件，查询参数在[api参数规范](parameter.md)中定义。

某些情况下，我们的查询并不只是在模型的记录查询，也会有业务查询，业务的查询也是使用`GET`请求，对于相同的`GET`请求，无论查询多少次，结果都应该是一致的，并且不会改变数据。比如查询班级数学成绩的平均分：

```
GET /statistics/average?classesId={classesId}&subject=math
```

这个请求是一个特定的业务api，并不是查询某个模型，一样需要使用`GET`，同时无论执行多少次，都不会导致服务端数据变化，查询的结果也不应该变化。

---

## 需要写数据的业务操作

关于对数据的操作，restful的参考规范中基本只定义了对单行记录的操作方法，在实际开发api的过程中，我们经常遇到各种各样需要提交数据的api，并且这类api并不是只针对单个模型的记录的操作，而是一些特定的业务操作，这一类操作，品高api开发规范建议全部使用`POST`方法，比如，某个学生因为生病申请休学一年，这个时候需要我们保存所有学生的学籍信息，并且将这些信息设置为暂不可用的状态：

```
POST /status/student?status=disable&studentId={studentId}
```

这种业务实际上是要修改学生的状态，但是得一并把学生相关的资料进行处理，并不是单独的记录处理，包含业务操作并且会改变状态的api，都需要使用`POST`请求。