## 删除数据

删除数据，使用`DELETE`请求，我们同样只规定对单行记录的删除，对于多行记录的删除，同样是建议在具体的业务模块api中定义，但是一样使用`POST`请求。单行记录的删除示例如下：

```

删除一个学校的记录：DELETE /school/{schoolId}

删除一个班级的记录：DELETE /classes/{classesId}

删除一个学生的记录：DELETE /student/{studentId}

删除一个分数的记录：DELETE /score/{scoreId}

```

删除记录的情况下，一般不需要特殊的参数，但是有些时候，由于关系存在，为了保证数据完整性，需要通过参数指定删除时对关系的处理，这个参数规范将在[api参数规范](parameter.md)中定义。
