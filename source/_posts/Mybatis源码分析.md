---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSYGZRFO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCCaSlLG7Vo8VEOaKfSRmuk5MxvW1oHPni4iLV2H7He%2BgIhALosaYuhnmkpdmeoEnmWVNHARDS32w3cnkC66IDSEaGGKv8DCEAQABoMNjM3NDIzMTgzODA1Igx5j1P4GS%2FGgoQsW48q3ANrzgIojJx4Coy77%2BvSGFZFjjKN25tzqaTET6NbXaMgNee2YjQxWwgMAK1h9K70s5Tc%2F%2BySVTZDGAD%2Fnoa%2BAfdluDJ7cJIQJRJnhRVpxuYWftJY1kojp%2FPzB%2BcjvSh8xCrQ8Hz%2Bvh84%2Bzmnq241pMGZ7fAeXVLFHfuWvGZFsJDu0r3BrPadW0ZJSPMCVSdiVW0175ywORcW8zf5yDSLDVHHzuT46cWDpOAPYv2NJ5n8uTLS71eH7IEcLaO06WeFXF3DdUc3jCii2dVh3J6c9Eu%2B67vG8mvfN%2BpGw%2Bv9StZ9%2Fyroag05TVpuUNXuvC41SZ5%2BUFb9M3U3W9BAiZ0oVptvyhITOFsKWO%2BIbR1eT0NSgwR%2FeWbwkXRSQ649le6YdEL6mkUIvwik1Eh4Oaz0nDWrSfvmL9OcA4Ek2ai7uREdHh73krH16GZowLtoAiOkD5S5xirQ4LI4Ir1FAArDFMBGwPqs9dUKosU4oWTB5uxd6wlo8Km3RhNipKM%2Fi74lAHfickIu9wcvoOs8c6k7DQHtffXI08J4f7EV2vVVbQQ9X7dmRYYKZbbH1Rmv322TVzsga5hXkFwhrVQ9ZzL9GNh2tZM2Bpg%2BXxMlZnaLdD6Difl5ChlFPhaPCAG%2BazD8uozJBjqkAdtN2snkw2nhn8EXZAy%2Fs4rzWB9XtucGAn%2BsCmedgLvALZmXxukbwYkmSd6CpafJboLKLaQsRpIwOqPV3htDu1CiRd0RR%2BZhrQCdffsVFZELYljfBWjPjyUMY2Ay84RX36GORhqQde8GYSRVJtM9H%2BgdDrhiC5A6Kr95RERN4EK%2FHZINGJNMgjItZZBpMe%2F89vGCsHXycIdARgsaRwaUUH0uQjJw&X-Amz-Signature=efe6e4645aa3d5d2b76316648305576d8b81c6be10218b03e8292a900cdadec9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

