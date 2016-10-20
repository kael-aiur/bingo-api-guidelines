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

> select的语法规范：
> * select是参数名
> * select的参数值是查询对象的属性名，多个属性名之间用逗号分隔
