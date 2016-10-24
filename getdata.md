# 查询记录

查询记录使用`GET`请求，查询结果可以是单行或者多行，可以支持id查询和条件查询，id查询的url示例如下：

```
GET /student/{studentId}:查询单个学生
```

如果不使用id查询，示例如下：

```
GET /student                                               :查询学生
GET /statistics/average?classesId={classesId}&subject=math :查询班级数学成绩平均分
```

默认直接发这个请求会查询所有学生记录，但是可以通过参数指定过滤条件，查询参数在[api参数规范](parameter.md)中定义。

