---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQAZ5URZ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJGMEQCIB8HIQc7nB0sKcOvyx7hzeBWpxF49LUQJ69Jd3lSsyKAAiA5ePVgTOaWYNRuGxMXC0xU1Izv3qPv1z%2BIAc6T8FxQWyr%2FAwhFEAAaDDYzNzQyMzE4MzgwNSIMmzOr2OboNGNI%2FLj1KtwDoqT6qKMga4qa3cn47s2OIauYGenmrOohMufHKpPhljCFqefB2wTHlF9pYTeVainsTpYDjC6igtxBMPoEgMrDYtcnyY5thDeZKL8akEO1MxMi9rSXXcxGh3mcIwNHOTtctINYgpx7uD%2BPGSfxVTat6MXROI29BH8WPUgVfOy8L72TRg%2BCgbUwbtF0SkvpwyWZSdLvPdx2Rq5qvBqdAsYzoNMRE1FICWgYigUk6P4JJrmoUiCc3l3ABR42S4Bm3Q0fs7Ud2AOWfCEzGvF0jMcvFuConeGCXz6wBwRgoh9CBBgi1VcTnE6x4OydqPMIWyyGWNACF4TAdjU0KcLEIw%2FJ9YJ0GDlAPUn%2BB2kndbk9pNuAkD41xGKQUkpfUJuKaS1LcY5a35usMrL9wULDVOLgQEkfkhtbPN8U%2Fo%2BFA7OfzyiD4%2BHK8sgWr%2BDn7wA45%2F4BpY3o66mEulwoJMYlz%2BZDssGyVgkKO1x2IJtR7rSxrZwZfbS6VYrVpsiwzoWFmzy4wQiMUnYw1EULFIvDS8tpw%2B5vrBdvBfB9b3RO7gCIZ48shahroe0y6ztfwczNyIbICRfGAjB3qHAbm4uPDOYEp4WxPnKqwgR7IWVYeaBaWxUK7t8JQuznwSNpklIwvLjVyAY6pgGxOg4dMcLwalVHOEoR1xwcsJtHZdrK1tyHzk6VSNTSXUdpaIhET4jem2WT%2BFCovdi4%2FJ%2BsbC2p6TT%2BQYY03qmBbXgKq6b8W0652hlkhQg5WK0nUiYg9BFPoRSmmWJENGjbRJldX6lshzhYmyJLzeEG1wUQvIiHdzAeoUvHIvlcYZ0OiznxNjSp%2BtwUt7zS61%2BFYfy8vIydtffr88PipnRUG03ZASmN&X-Amz-Signature=43a238d33a7358995508de79d84ec0e9afa4d899118e021c405ced8340ca2fbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

