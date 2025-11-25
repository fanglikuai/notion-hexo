---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHA6TGSC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD46kQ5uY3ylUtJnEBoc9Lm0Jp3KldXtF8vp7M17nB8BgIgSaRcK6y5RIRJ7nHvLEH%2FosgLb5qy%2BaVsrrLDn2YQtv4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDBivjQGysjdy%2BOneqircA6thQ%2FElPG51NAZmregjhn2T6D%2Bl2gYpYFy1ri1yAYAkgLlKaMzl%2FIpWOPF6HoJNc9kb1qdASNBFdMl%2BOzJv9oK6iZebk7Jx7VyuoaBpiTHknc6hjfDSoMaLW489oMaEkefg99hZc6eZgH2qVfcLqsAJYceTVWIVOvKkhHN92wSEUF3IpzYYrfHr1NZzBpMHTLLw5dZiK7XCtHueKH4Q0BuwSOMHDZ0dc5zd5Tqi6lRGtwfabnjNWQpYeaItK8bQ1J3ZjInr1rosmCCViX9c7vr283GOhZjyEFUIAyX6ILMcQ3eVs8DlRrSEJXanDcU2milNaWkjzJ4Wgh7gyWjCztaSId3xmh5LoySBhTlSU%2FL36QNLL1W1sDmgO8W%2BJxeIviO0wNr6JlccZkoFsp%2BMnvA5yfJjg8K8pDmus2tL9HWmUo7M5mQxu8ZEa760VX4FGF649rHqJ0gEzbD0Xoer3aHkttDh7D6CN%2FEcvwEqJhMqUdrsw5lHsDFkaYwlTg9iI8BAigvLYqoxRFUdONlOAO9qNn0YrXdKzlrNR7IbWnxBsJ0izWepaPIia2uUHNS50IUsCzoRU7BRh3tSFLWTuEQuuQ0lNl7bdF2ZQtbHkjSDFK%2FJgCAeGJ4hYxuwMMmClckGOqUB82GqkG4uHppGvOFPclh%2F0KOxu0kaLMEu6n0ifnu4PJZgcOA%2BuLfBuqIbbHmvT2DB9HXfgC2AzqIdGKmaWkw9YUgifk6mIykPlioXXPI0G17Q0NFJlDVbTWaaH4wljpGEECHJAb2U8D4qdHu4jAIhHYOBtIhg4Ps7txQragsVhCMw1kl%2BTfNE%2FwQ2nT6wiSHJmiZECFWdDDBiNFL0ONBYsHQM4iT7&X-Amz-Signature=c8054526c5930d82fa276283bb4b32a9ca15098218b9fb58751ebb8190d6820d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

