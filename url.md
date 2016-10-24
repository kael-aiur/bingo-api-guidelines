# url设计规范

api的url和一般的restful url相似，但是为了更加方便设计和调用，对url做了一些简化，下面是url的设计规范表：

|场景|示例|规范|
|----|----|----|
|url中的模型名称|<ul><li>/student</li><li>/primary_school</li></ul>|全部单数形式小写，下划线风格|
|单个模型的增删改查url|<ul><li>/student/abcdefg</li><li>/school/abcdEfG</li></ul>|在path中传id,参数值不做下划线转换|
|具有包含关系的模型url|<ul><li>/student?classesId={classesId}</li></ul>|不使用path的方式传递关系对象id，全部采用普通的参数传递|
|与模型无关的业务url|<ul><li>/statistic/average?classesId={classesId}&subject=math</li></ul>|url根据业务名称定义，使用全小写的下划线风格|

---

## url中的模型名称

多数情况下，api的业务功能都是对模型进行操作和处理，如：

```
GET /student/{id}                            :单个查询
GET /student?filter=classesId eq {classesId} :多个查询
GET /classes/{id}                            :单个查询
GET /classes?filter=schoolId eq {schoolId}   :多个查询
```

无论是查询单个学生的url还是查询多个学生的url，使用的都是单数的形式，不会使用复数形式，由于`class`是java的关键字，需要考虑如何避开关键字，这里使用了`Classes`这种复数形式来给类命名。

> url的设计，一般涉及模型的会将模型的名称从驼峰式转换为下划线式，如果某些模型的名称本身就是下划线式，那么可以不转换。

---

## 单个模型增删改查

现在我们以对学生信息的增删改查为例子来说明如何设计url。

```
GET /student         :查询学生记录
POST /student        :添加学生记录
PATCH /student/{id}  :修改学生记录
DELETE /student/{id} :删除学生记录
```

单行记录的操作，**url都是在应用的根目录后补充模型名称的小写单词作为url的**，如:

```
http://localhost:8080/api/student
http://localhost:8080/api/classes
http://localhost:8080/api/school
http://localhost:8080/api/score
```

从上面的例子可以看出来：

> * 对于单个模型的增加和查询，url是模型的url根路径，通过`GET`和`POST`来确定是新增还是查询
> * 对于单个模型的修改和删除，url是模型的url加上记录的id，通过`PATCH`和`DELETE`确定是修改还是删除

---

## 具有包含关系的模型url

**规范建议不使用这强关系的url，对于模型的操作就直接使用模型的根url，而相应的关系模型采用参数的方式传递，如果是强关系的话，使用必须参数，非强关系的话使用非必须参数**，如：

```
GET /student?schoolId={schoolId}&classesId={classesId}&id={studentId}
GET /classes?schoolId={schoolId}
```

具有包含关系的url设计规范：

> * 具有包含关系的模型的url也是应用上下文的url加上模型的根url
> * 与这个模型有关的模型参数采用参数的方式传递，不使用url的方式传递

---

##  与模型无关的业务url

**对多个模型的综合业务，这种情况的url一般以改业务模块作为api的根路径，后面加上具体的业务功能**，如：

```
GET /statistics/average?classesId={classesId}&subject={math}
```

这里传递了两个参数，一个是班级id,一个是科目，目的是求指定班级某个科目的平均分。

规范如下：

> * 与模型无关或涉及多个模型并且没有主次的业务api，使用业务模块名称(下划线式)作为url的根路径
> * 具体的业务功能名称作为拼在业务url后边的子路径