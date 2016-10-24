# api查询表达式

api的查询表达式是在查询模型记录的时候使用的表达式，用于过滤和指定查询属性，排序等等功能。

本规范指定了查询表达式的规范，api的使用者和提供者建议按照这个表达式规范来使用和实现api。

查询表达式功能如下：

|表达式|作用|示例|
|----|----||
|select表达式|指定查询属性|select=name,age|
|filters表达式|指定查询过滤条件|filters=name eq Tom and age lt 20|
|orderby表达式|指定查询排序方式|orderby=age asc|
|expand表达式|指定关系属性展开方式|expand=students(name,age)|

------

## select表达式

select表达式的作用是指定查询的属性，类似SQL的`select`关键字，我们先来看一个例子:

```
GET /student/7D82fxxw1jn                   :请求1
GET /student/7D82fxxw1jn?select=id,name    :请求2
```

请求1返回的结果如下：

```javascript
{
    id:"7D82fxxw1jn",
    name:"Tom"
}
```

请求2返回的结果如下：

```javascript
{
    id:"7D82fxxw1jn",
    name:"Tom",
    classesId:"7h72GggUMsn",
    schoolId:"7hUdfgwURaH"
}
```

从上面两个请求的结果我们可以知道，select其实是指定了只要查询哪些属性即可。

注意，无论是查询单行记录还是查询多行记录，select的使用方法是一样的，区别只是返回的结果是一个json对象还是一个json数组对象。

> select的语法规范：
> * select是参数名,表达式为参数值
> * select的参数值是查询对象的属性名，多个属性名之间用逗号分隔

## filters表达式

filter表达式的作用是对查询结果进行过滤，类似SQL中的where关键字，我们先来看一个例子：

```
GET /student?select=id,name                     :请求1
GET /student?filters=name eq Tom&select=id,name :请求2
```

请求1的返回结果如下：

```javascript
[
    {
        id:"7D82fxxw1jn",
        name:"Tom"
    },
    {
        id:"7hvq3wggMsn",
        name:"Jerry"
    }
]
```

请求2的返回结果如下:

```javascript
[
    {
        id:"7D82fxxw1jn",
        name:"Tom"
    }
]
```

从上面的例子我们可以看到，filters表达式是通过一定的语法规则来定义查询过滤条件的，这个表达式和SQL的`where`表达式有些相似，但是由于html对特殊符号的限制，filters表达式是通过*操作符*来指定操作的，而不是通过符号。

filters表达式中规定的操作符和作用如下：

|操作符|SQL|说明|示例|
|----|----|----|----|
|eq|=|等于|filters=name **eq** Tom|
|lt|<|小于|filters=age **lt** 20|
|le|<=|小于或等于|filters=age **le** 20|
|gt|>|大于|filters=age **gt** 20|
|ge|>=|大于或等于|filters=age **ge** 20|
|ne|!=|不等于|filters=age **ne** 20|
|like|like|模糊查询|filters=name **like** T%|
|in|in|在指定的值内查询|filters=name **in** **(**Tom,Jerry**)**|
|and|and|表示多条件间的且关系|filters=name **like** T% **and** age **lt** 20|
|or|or|表示多条件间的或关系|filters=name **like** T% **or** age **lt** 20|
|()|()|表达式组，可以用来改变表达式的优先级关系|filters=name **like** T% **and** **((**age **lt** 20**)** **or** **(**name **like** %y**))**|

从上面的表格中我们也可以看出，操作符之间是可以组合的。

> filters表达式语法规法：
> * filters为参数名,表达式为参数值
> * 表达式的语法规则和SQL的`where`表达式相似
> * 操作符和操作对象中间需要一个空格分开

## orderby表达式

orderby表达式主要是用于排序，类似sql的`order by`关键字。

我们先来看一个例子：

```
GET /student?select=id,name                   :请求1
GET /student?orderby=name desc&select=id,name :请求2
```

请求1的返回结果:

```javascript
[
    {
        id:"7D82fxxw1jn",
        name:"Tom"
    },
    {
        id:"7hvq3wggMsn",
        name:"Jerry"
    }
]
```

请求2的返回结果：

```javascript
[
    {
        id:"7hvq3wggMsn",
        name:"Jerry"
    },
    {
        id:"7D82fxxw1jn",
        name:"Tom"
    }
]
```

从上面的结果可以看出，我们可以通过指定排序属性和排序方向来修改查询结果的排序。

> orderby表达式的语法规范：
> * 参数名是orderby，参数值是表达式
> * 排序使用asc或desc指定排序方向，默认是asc

## expand表达式

expand表达式也叫展开表达式，主要是用来解决一些需要额外返回的补充信息的需求的。我们先来看一个例子：

```
请求1：查询班级信息: GET /classes/7h72GggUMsn
请求2：查询班级信息: GET /classes/7h72GggUMsn?expand=students
```

请求1的查询结果如下：

```javascript
{
    id:7h72GggUMsn,
    classes:1,
    grade:1,
    schoolId:7hUdfgwURaH
}
```

请求2的查询结果如下：

```javascript
{
    id:7h72GggUMsn,
    classes:1,
    grade:1,
    schoolId:7hUdfgwURaH,
    students:[
        {
            id:"7D82fxxw1jn",
            name:"Tom",
            classesId:"7h72GggUMsn",
            schoolId:"7hUdfgwURaH"
        },
        {
            id:"7hvq3wggMsn",
            name:"Jerry",
            classesId:"7h72GggUMsn",
            schoolId:"7hUdfgwURaH"
        }
    ]
}
```

这里我们可以看出expand表达式的作用，即指定展开某个关系关联起来的记录信息，在本次查询中返回。展开参数还能指定只展开某些属性，比如：

```
GET /classes/7h72GggUMsn?expand=students(id,name)
```

查询结果如下：

```javascript
{
    id:7h72GggUMsn,
    classes:1,
    grade:1,
    schoolId:7hUdfgwURaH,
    students:[
        {
            id:"7D82fxxw1jn",
            name:"Tom"
        },
        {
            id:"7hvq3wggMsn",
            name:"Jerry"
        }
    ]
}
```

> expand表达式规范：
> * 参数名是expand，参数值是表达式
> * 表达式就是关系名，可以在关系名后使用括号，括号内指定展开属性，多个属性以逗号分隔
> * 多个关系展开以逗号分隔，如expand=students(id,name),classes