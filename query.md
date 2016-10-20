# api查询表达式

api的查询表达式是在查询模型记录的时候使用的表达式，用于过滤和指定查询属性，排序等等功能。

本规范指定了查询表达式的规范，api的使用者和提供者都要按照这个表达式规范来使用和实现api。

查询表达式功能如下：

|表达式|作用|
|----|----|
|select表达式|指定查询属性|
|filters表达式|指定查询过滤条件|
|orderby表达式|指定查询排序方式|
|expand表达式|指定关系属性展开方式|

------

## select表达式

select表达式的作用是指定查询的属性，类似SQL的select关键字，我们先来看一个例子:

```
请求1：GET /student/7D82fxxw1jn
请求2：GET /student/7D82fxxw1jn?select=id,name
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
请求1：GET /student?select=id,name
请求2: GET /student?filters=name eq Tom&select=id,name
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

从上面的例子我们可以看到，filters表达式是通过一定的语法规则来定义查询过滤条件的，这个表达式和SQL的where表达式有些相似，但是由于html对特殊符号的限制，filters表达式是通过*操作符*来指定操作的，而不是通过符号。

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
> * 操作符和操作对象中间需要一个空格分开
>

## orderby表达式

orderby表达式主要是用于排序，类型sql的`order by`关键字。