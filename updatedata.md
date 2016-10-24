# 修改数据

修改模型数据，使用`PATCH`请求，如下：

```
PATCH /school/{schoolId}    :修改一个学校的记录
PATCH /classes/{classesId}  :修改一个班级的记录
PATCH /student/{studentId}  :修改一个学生的记录
PATCH /score/{scoreId}      :修改一个分数的记录
```

修改记录的时候，需要修改记录的哪些属性，需要通过参数指定，参数的传递方式[api参数规范](parameter.md)中定义。