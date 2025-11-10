---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOQWMDDG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQGZYemQTgo7AUw69HhRe7TQzY8rlvV9sqzK3oN0wcbQIhAO6m7yN8FHbd%2BqfQc3eMM7OSjsdV%2F68RqerYv%2BDxgcg5KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJw2sl1n2cdriZ5MAq3APH9TRyVqGqk1zjER6r0MuuAh6KJTJ9gXTDFCr6GLg%2Bn5X%2FyJIzzJ%2FaRuMnSA%2FqJMhtGXiSW8Edj42tKrR3gw1k24x6K5nt3AMiayQ58mjWXXHsU3BgRLQrbybFgNEIbDsbrDOxmROaohaw%2B4wQsyqpO0PzvPKPy9CV204q0GECgKuiyLKiNg1d71NcdKV%2FdYf9bYyUWo6KEdpBYIR8qkytzhWVaY%2BdzRkoqg03REl8eIR0g84BzrGlJdgv0X5D7nauGL6YPhuQ4C2pC1TNTz%2BKjWlng7PciON9iS1j1N%2FSLi3bIUnz53GkzGS76j5WZbkw46hOeq0ugHrzYyFwyKjpcl3X%2FknNCmjS7YRICfcSgm7Dku%2BqOwF40%2FhzgRVEktNzI7iTu8MWHlUEC1E0YGtZKCgKL4ZOCbyMhryXN9FfqcsRYj%2BwsDLqcgEx1rm0M21dnS7wKkfZPX4Sp7bJfZH4%2BSg2l%2BJLWcxYd5XfdwieronHnhCSvZr6rvh%2BU7IQu7F3rRxG%2B5xuG1RpRJA5WmkIlkoHP%2F1KL4tYgPgKQY4Pjxf5Z7NI5v9gRc%2BGTpyvWXJ7Z0qzo4lMNvtuztroyD6mjZGG3QUs4Kqod9q5GrKpxRt3WoBRTSiKOz2ebzCu7sTIBjqkAdXrE%2FmHVYKF6qeXZA6DLIa%2BnkQgKNXhQofnAQNAHocvsvB2Uahf%2B6digqAlS7xbBAoGuygJzb0wIYoS0OATTRpkaYSPkehUKIA1WbiErt%2BTEHAJZY%2F6EuXUxZttAiZJScb95Yt9k1oT7m0XYpf0dYsVuBW0hk5HyMvTe7WgGdHalaD1ZXILYZal%2BZI9LkD2OlJiV4%2Fla1LaKNSm4ZFlqgsEzJcv&X-Amz-Signature=3f175ab8fdd5ece7bca623d36461fe92ee8c81c686c83edaec4f54a20f8dc6ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

