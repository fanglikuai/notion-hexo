---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQYP3RYT%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJIMEYCIQDDwR7TrvaHFll%2B9gOdZ3sUTgjVVDtllty5Zp4Qda4kbQIhAKGeGZJAWzAie3tD6Iij1lJhn8eY%2BwX%2BVVgXidc3SZr7KogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzuRHoQbblXoJB71L8q3AO0VWp1mlhV8mNLO91BnyYZnzlNTXkMcrS%2FzxnAYkWvv2oKwRmrP22wMhSp37I7Z6InQFZf9DBY5JGG%2BKKcQKjB0wsc0WHcqGE4JZOyJ%2FyUThZWlnPM%2FhNbYy629P%2FOIhp6VCb186j6ReC8DIvrMd1CHIGzzbdSCN3MU4Q1OQNXVkZjTxQcUoFaoRHVs35oGJ7TWyqbidPPV5sXmngJ89zq%2FzyWCXTWHC%2FgGqtSxMplTwmt%2BwxRmnBIKTV%2BAPScEZOiI9JrhH2nTc3uukqD8rk%2BJsVhGN1oXgh0zt5CZzHVUb6OMvOjVjox27Ij7LUCh4UCSZ3kydqRne8qG4%2Frjq7Zd8Zx4vO2o8kb3OvVKJs%2FNb6uBeJSnwfKb3HtMal0JMygMddsXiXZUjydkTuVoOry5fZQ2e54djdICkvZnCsA7Ff9Tt02cowZi%2BLOoDiG8UMjrlkdaTHvdGum1tKYH1pAvw7%2B2MzDAWtJiA9%2BFHiRrZgDnJa6L7o2ZhRsnHHzkgWKav9Yrt36nf6eUFfAqrg6lMi1OIpKv7wPdUMgIm4%2B%2FnbONfPB%2B%2FBI%2FCinRLKduMHZVVqydSC7QSoP0hMuY%2FwV2vnrII8bMk1ndpbdsOaQGcnLrYUnOWy3w8lTmTCJganGBjqkAVGTr1QtMt7Q3kplWhW9V6VAdpMx4xavceByQIQ9%2FFIIMTmQXTjRFlEHHguOHmp9mC2XQ2mKXzLlYKIxEHD%2FMHenj9VYZ9xyYoyZnMK3Pju5GPv19OLGdPNK1ZgniqmQYuFYCjt5LNXCm00ulob3C04x4Z9i7C3CtXO2MFooTPODOf01SWA%2FGqdzncSIQABY7MKAD6yg2Xwj%2B4FG9HlNSl1prIDM&X-Amz-Signature=5b1fb67b0f021f29685b4bf739383e413d9ae631564ba2631870880f5aa1cf81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

