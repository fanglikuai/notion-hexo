---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSOLUKZX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9xpGBJwolkcupLCxomIPfb%2FR1tyB6W0rKxjN%2FfxdbBwIgaVHNTI5fi6WodAbmJydfCUh%2F0h1DGd%2Bp%2BO7yS5TGO%2FYq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDAUexFlmW5CfJ7x48SrcA09P7sSXOXz8tZ%2FfjWYG63%2Bdnm8lpWTIjk%2Bo0P3DegjDpSmJ%2FohIinnWHtnsJaoBRV3WMKoZodGYwcKk5g6M5xk%2B4XscH7VNvM9VYgSTlvfkzDEINdrpYzpFv1QhvZ0GdH4sRSqtDlDNj7tmeGfGJxIhVyuBeTVoSTas3yCSM2J0FJJCNOhliriCtd7PZgb1%2FSHdI0nfF%2BGbFtezY91dk8SWy2ng76qeqDeusLmwM3QaIrq%2Bkaql88vZc2JNMitZMs5%2BS5YqQ75EehH%2BztjzXjCp5rXpjTO0PZV5P7EqBkfNl2LfVmpWZ7YV3DdVPi7EJYG5K9ST1NkqdYPjjgjyfHtQxPLuEUwlQ2XcZnrzSXVpJFO8UhwFIkZBXslVm7vy7rVkdgZG5%2FzhbMGRqzlSehc3QQxvv7ADa2AxolRgcHghqWxvo9fw15VMLmkOJNN%2ByoAXaCb1l393LOU1Iac4yH5V9c%2B1%2B4gnCyiKxYmONGsrDwB7ze5SJTaAFgjZgPqQ0Tj8YsIvyUs43%2FFP75lfzw6g1K5hjFgxXW%2FMnR%2FyMTyLz9reZCy0c9CUeP8Iq1BD6AeihKxjCZ6v4VVq8%2BAMn62Lv%2BT%2B9yF6vSAVTkGLSlQ5lEMR%2B%2FGVRiyPmR8GMO3Ps8cGOqUBRdV%2F%2F2mwX4OFJrAnORxLCkAz8wX0urpBlXEmQhkOOA%2FdBqZVtPflH4khxV%2FYe89rJeIQiQesw6%2FtFFdPi2NMZWUUUQiksdamvaBUNtxMIRgZcWgSj6FRES7eoyGb1AE74sSz8S2Vls4KNhydvWpa6Tcr7qDpsOn0g9Ix%2BPQeZunuzdTBMnjg%2F1Yb5CJBBjDzxveOiQvyeLuOMu4JnojmqtsnbXlD&X-Amz-Signature=b1d0fad28b5892140b3576a7a1b0ea29272ba25ba709ee7b178a736f2c04b77c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

