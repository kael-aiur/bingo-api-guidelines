# 创建数据



创建数据指的是要创建一个模型的记录，我们规定使用`POST`方法，如：



```
创建一个学校的记录: POST /school

创建一个班级的记录: POST /classes

创建一个学生的记录: POST /student

创建一个分数的记录: POST /score

```



创建记录的时候，模型相应的属性通过参数传递，详细的参数传递方式在[api参数规范](parameter.md)中定义。
