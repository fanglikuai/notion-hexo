---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPZQHXSX%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDbmyMJRRhkSGr2eCy%2B1ytgBF%2Bd5dUK86lQzxEBWdzELQIgf7sj19VPtPQkfGXP3abwJyqGRH2n60IPSg%2F%2BjAPhaGQqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLK%2BBycQEqORIhsPLyrcA0b5GmOIFTX5iYCs1VCUNZFALbVoV2QeF6YXkLbpGeXHWR8LcxwaLmfRY2ohvHSBuxTlA2kqNmKjcRMyr4%2FRUeS008D7YGKJQfXJ9YabfyMgLJsuHCZGa0mq10%2BtwwyStH0hjhA%2Fohu8XK6KmHCV5aLMT3XM%2F8SddrkOaIgTFPYZWsiuSzDvpcyIRxh%2FSoIuLaIKwM5NdeDZ6f%2BN3k9UtOuhoAgTEzcrajH45h3ZpsO0vI9mtezI6lhWtwcIRTzZq%2BjQPLsXwE01E33s3ErPHV4Z9kDextxyrGKM2srM6zRpTg7xSENsrtJm84wwFRhuGYcfmEvSBqk2IlKBfPJX7b63O475FmwiYqi68ETz3R%2BIqlncGEInk%2BDqeMcqtAjtHABZa9D%2BwtfVifB9JdV2dQu6rq0OgyNoXm86VBsrRKxLwRazNJbQq3VV4PG5h%2FtAb0nJwQ8Aa5mirgLtanLi%2FHmwDydO5m5WuUXlks%2B8wMOcrbbw4Ftxb0EN5nAAMaNgET24QhtO3B%2FRfhSjnS7hy7eGPco%2BskHJQ41KRh5e2WrNwLCyursZX6a3y2WsqnO7OFbeYvcCPdBY6doP8jRlo64zOg06gn%2BPpRzaOPo6%2FVeAZMO6aUgWY%2FOsej5MMNXq8cYGOqUB0jip%2B62j4YmLRZYszSZxTBFAjyC6SUPy9iHfdvGUFHXc%2BfOJOxAm5tLRaqe3PfSx9MYaRtd%2Fo9ZRAUPo4fOi%2Fw8NVJHB3hZuit%2Fz3V4ZKRlc8PGH0%2B96ctrsFSHDAYZNg99ezHG6D0LKe31hymz%2Bo1vjcMO2nU55vc7VdAeZuNP8MTJMsaKFJBPCSYlCmisKp2P%2BflnpdfGSCtT%2FJLM1m4D6EYqD&X-Amz-Signature=df3955d3bd82cde4d87b4d54d45b2fb05a58d2552a94a60964b053518717f746&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

