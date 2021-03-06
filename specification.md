# api开发规范

api接入网关之后，网关的路由节点会负责api的负载均衡和运行监控，以及之后的日志分析，另外把用户身份也校验了，api只需要关注自己的核心业务逻辑即可。

当然，为了更加方便网关的客户端使用这些api，网关的api设计必须遵从一定的设计标准。

在介绍api的开发规范的过程，我们需要举很多例子，为了方便理解，我们这里假设一个api需求，后续的章节举例都以这个需求来作为例子。

假设我们需要开发一组用于学生班级和成绩管理的api，这组api需要管理学生的信息，班级信息和学校信息以及学生的成绩信息，使用UML表述我们的对象模型如下：

![对象模型](/assets/example_model.png)

这里一共有四个类：

```
School:学校
Classes:班级
Student:学生
Score:分数
```

这里我们可以看出:

* 学校和班级是一对多关系，并且这个关系字段是班级维护的
* 学校和学生也是一对多关系，这个关系是学生维护的
* 学生和班级是多对一的关系，这个关系学生和班级都有维护
* 学生和分数是一对多关系，这个关系由分数维护

后面的章节中的所有例子，我们都会基于这个示例的需求来举例。