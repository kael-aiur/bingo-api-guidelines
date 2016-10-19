# url设计规范

api的url和一般的restful url相似，但是为了更加方便设计和调用，对url做了一些简化，下面是url的设计规范表：

|场景|规范|备注|
|----|----|----|
|url中的模型名称|url中所有模型名称统一采用单数形式||
|单个模型的增删改查|采用模型名的下划线式风格作为模型访问的url|增删改查通过不同的请求方法区分|
|具有包含关系的模型|每个模型有独立的url，不使用url嵌套的形式||

## url中的模型名称

多数情况下，api的业务功能都是对模型进行操作和处理，这里我们以对学生信息的查询为例，一般url如下：

```
单个查询：GET /student/{id}
多个查询：GET /student?filter=classesId eq {classesId}
```

这里我们可以看到，无论是查询单个学生的url还是查询多个学生的url，使用的都是单数的形式，不会使用复数形式，值得注意的是，如果是查询班级信息：

```
单个查询: GET /classes/{id}
多个查询: GET /classes?filter=schoolId eq {schoolId}
```

这里我们注意到，班级使用的是复数形式，这里是因为`class`是java的关键字，模型的名称尽量不要和关键字冲突，一旦冲突的话，需要考虑如何避开关键字，比如这里使用了`Classes`这种复数形式来给类命名。

另外，在将模型名称转换为url的时候，规范规定的是**将模型名称从驼峰式转为下划线式**，前面的两个示例url，由于模型只有单个单词，因此我们看到的是直接转成小写，如果将学生的模型名称定义为`MaleStudent`，那么url如下：

```
GET /male_student
```

## 单个模型增删改查

现在我们以对学生信息的增删改查为例子来说明如何设计url。

```
添加学生记录: POST /student
修改学生记录: PATCH /student/{id}
删除学生记录: DELETE /student/{id}
```

我们注意到，对单个学生记录的操作，**url都是在应用的根目录后补充模型名称的小写单词作为url的**，完整的url例子如下:

```
http://localhost:8080/api/student
http://localhost:8080/api/classes
http://localhost:8080/api/school
http://localhost:8080/api/score
```

从上面的例子可以看出来，**对于单个模型的增删改查，其实url都是同一个，就是模型的根路径，然后通过不同的http method和参数来区别操作的**。
