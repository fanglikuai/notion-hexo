---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXWZBDOX%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiL8b0DsNG1%2BWrwIEWg7AL8eR1B4XIElLDxBtn7M9TbwIhAMYXj6%2BHAurGVEDbv6kSKozoYAEC7xeWTbypT00ybrYtKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxv23GSBHpiWvSATUcq3AOVLcGRPl5RcoT0fgjRwoiFzWso9MJtFgOdIvMMX%2B%2Bsicas5viMSDvM7614FqqXr8WTEuBvkVlQOl3EQ2WTioFwxOsejjMJ6g9Z9IzqMM%2B%2FDQCIHQaM6OtsrpQ%2FCE6QzvXhZvmOf%2BhxyijUdJ40NbwZX2uJEm2lF2Xyg0FjKy%2FX31WbLPUP29KlOnUJLj0tIF6kPHF8%2BFqFTpWk4BSIObMtTEG1O0Ln2Hzy6889XKuesW%2FbPs7RtWM9JE3xYv04NnH4475tSTYBZ7tdoGfV0Dwxg2fXoNCDXjHsNH9k%2B79toG1kDZy%2BIhquQhExl0WX6lZovVx9asB2NY%2BFAtrkt%2Bsx3957GI1L4mKjdqI7GutKBVxGskqu0GnVtCTeC3LlirWY42JflFHEQQKewbQ8fhqO%2B1nLOB48q6mOUxw4zu9djCwi132YGxPlIHgCtBuOY8ayVShmpT1urRwbPyj9FjFEtCQjmpdMWsAIbitTSSGNWLDsJJHhOhUOGJoufmHERHDZL6%2BOCjfN1NLuCN43eQAfwQpOhFx7spGVZgKBBhykUngfeAiEQqf422LfMRhk1zUGl8PMag2UTdBLhesGB0DhoKKUhbDAW9%2FQ5HY6YoKk%2Bq3dVbN4AAAJ2rw12DDPgbXIBjqkAUGOaB%2F35QOzzY3S0q6cDEpPEdXkYiTPSzQbqj8oo4jMvvPMkWDCJMtU7xVG6j3D716ZFgSCzh6ju2s016ceBo4Jwy0N1bMcnYW9%2FZ5w%2Fy0vYDf8jzLM9U4zSTP9MUN9TpznxBJlh1FW5Y7GIz1SQiq3u5WhyqfV5o7so%2BFv6Flb0p1WY6dtK2WsQVY%2Blc276O%2B9fLzW3NVRx60c3MXjSOBkkdz%2F&X-Amz-Signature=3d78d94c012770df98e548d70ce0323dc083da3dea7e9830a570d0e368e32b30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {

    public static void main(String[] args) throws IOException {
        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");

        // 2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();

        // 4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);

        // 5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

        // 6.释放资源
        session.close();
        in.close();
    }
}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

