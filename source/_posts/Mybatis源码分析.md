---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUGPYKH5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDG8WWsSk9duX5dnQ5VNcgDdUA6L2etV8XuUO95xi7oFAIgdM95T4T4u1k%2Bpuh%2FfeomG5LPFby8Bp4RaKWwRReQ%2FoMqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFBxqoBIRHimpcqQPCrcA15svr5T%2FrbnBEMNKfjUYVGD4BIDqBsd%2F2tDu5jkEsuHsBJxS%2FI%2BZ3ULOwbCywTPbLu3w0E9IBMX%2B%2Bq4uQkdbLRgtJ0ZuCBNxKeRygk66Ya9ehxyHhdCgK16%2BiDZb7xcqT%2Bkdg8p9k2NojaPdV3FSHdTjdqGCc1ebr3SC7sk7A6bLsclwiivPAqoFmtwwSpDBmuxxr13MHFkKiaHyx3paM7WYolFs7Htp9qhCDgv19f5x9PmPW5m0rcMJPLWfD8hQUm9WL%2BvNgnU5dl1Adp0mYSM9UoKuCTRnO%2FhAUESZdBfXJNkWF9Ef7dNBv0I9slc0B08oxrUYRjO33n%2FdyXbZDCqTHYKm22qXNKeQG9j1LzQWCObraZ46QA2nTrMs2Zh56GTK7QxaC%2B8zvFDS0dh0krfSREsBi6%2BjdQbzOG9oA4ejqyKtIlpongq0jib5wQqiFyGkdDrQjpOrZHuWFcQ2iwAvBzymmC%2FhULInbDAnXaiEakvuvhC2ukhhWx%2BJsJD%2F%2B0Jx8ZTzrHKqzsOiUlM08Met7ULjSwQyx%2BZrHp1PtNe%2BQ1DpnvF84UqXEp2TJwZUi89oZZ4fWIUm7jBqPdRNLTGhqJat0GENxRVC8f9UKkjNfT1jTSA3dUI%2Ff4%2BMLScicgGOqUBEYProqdYR00LhVup8t8FmIWCkEgkulz8CHZvSYya4C8Kcpqd%2FiqwIxmwjhVMRAcmzeItOBFLj5oeTGKau%2FlNHWlyujiZ0u47A2WD7N6UtJPSW%2BDp3nMwBgORVSkYv%2FxI%2FdwKa0GApGy%2BafgwUwdC4zCEfh%2BftEIgydi%2FkLVJkjUoU7LlBzwV1vzgjIf83iRUEKguhiazrTkH%2BWC%2FNeM%2F4ZKrPRDk&X-Amz-Signature=e94026f79c8342333b02c0620e8b48034f8825344af39bf0509ee815ed4bd32a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

