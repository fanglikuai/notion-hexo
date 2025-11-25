---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4IZ3IF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAyZmNJ%2Bwqo7Cd6GhEvI8EvGb7ZOXV8wcDyPxzIRXdw4AiEAuLMy8CcJl0JOZJjo20YL6%2Fa8u5owmdA1QZdLfVNT%2Flkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDPjo6%2BJQCFVbzh2qECrcA874dUElUaeKjCAqNv%2FCTZgOGIzvtr52VHavvrsylsgNRmnvcWxiQBfHahOKShD1Zljdp9eqVlS%2F5lNtdmuShOa7%2F7yq92bmJm3Zwn3ndX1VIng8Nw6Kd5IrYuZCSY5Z9lx6Jp1qrMRjbvekmTBfGW2ZJRHla%2B4nXdV32Jyb9alD3lG1mjTlh3wWNL%2FSiyIFEOa933CSnU%2BZ9HreTgfbfvlFR3xW%2BgY4F4JwqTCy0wZvphs33NUvDTX%2B6gx5zut8BqgCTLnHe1ksaQTtu1SNbsFy4cbjI3hsCNyKFeU7hlRYkOJmfcVfEoD2ve8NyVHPLI5Hm%2F7b44xbNIEHqdS5uC89klLmJEP7mcXO3iSzLAUcK9DYh6u9EWMiRQZ0L8aFQFvFg3TaSAQ2HtCOV39MMgOyK89Z59PkrDf0Rfldu8wkLs%2Bsl8XB5hVhqZQy6nTi8Rt6I48Q%2FOfaY1frKIuZps2iuXL2pVrBPyP1uAYS52CZy%2F0CxMzg%2BV%2Br%2BkYvjMDy3nOC143%2Fh%2Bh%2BbvuWGQ%2BsDLwfCNKThYQfPB6XjBtJCNRX1BkaJnj1yhXxG0qMpCSsHI00tqqMyfYqWREHT%2Fiubi8uOzKFHgazDIOv52Gqr3Ug84Tc1DiEB83jXJmJMOe5l8kGOqUB%2BVLMgN5dkXNOHWPdhV7s1WZXEXZk789%2BlQTScohonItLtwTR4%2F06Kwr%2Fpey5y8iGoB1FQjs0g9qlEofmhBmGCtl0AzeqfmyqJ9BUZI3Q2J61x4YkI2TOxmVHFzmPUtPzBxRvEby445xOxSja5XpnPhwH8nEA%2F3aC0THp8Hz4HF2DS6O9dZOIRMBDO3fFiuniXBoCwR%2FFWSaJdcWWvtNrMggpydGr&X-Amz-Signature=36881eea08f842c7bf6cb52272df020602338ca10684a7bd7910ee1a665c537a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

