---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSYXVICJ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDopm1Zh6gisFfXB7xztnXv%2BFnInBbAjVWdMOOVMiEDFQIhAOto0oYp56MkJfuiZTHUX8R8%2Fer%2FoZLNzSwSYi1ThixjKv8DCFQQABoMNjM3NDIzMTgzODA1IgwwV81IG%2FklNoMfqPEq3AObwWQ%2B1Zoom%2FODRIPDO4f0eRPwBAbf4FTnLZPeBjCVbjheLz728tEsWL9NBTqhqpLrS%2BuJ3AQPZOTfy4Wl0K0VJzm7RoP%2F5LEhabanP4yyjdIpKcIRK2LWxqaUmmx7S6aTGsHp5od%2FbAMMI9lIw4S2GzKAVHpJb5OpEHrNfZczeIkQqz6PzEHAgav0js0ZyMkRBSxeb%2BhXxIg70PpJ4VI4X9JVu3tBEE2hSplbFrsZl4maV6ilMHg9hrVjSIAQS6lA69dqy67U6yuTXRbJJPYq8OCxZApOKNfqxjMEm4MLdxGKv06Yk2ntpZPBIT4hNJj%2Bau9qzrjJC9v8MpetMLYBbJxYmU1GfrcH5FTFg0t59Amjm8aUEzuwq5F90ZUlCBFEx56Im8qYDx%2BrXXoJp5Rx8AIIEoyzIbw4olYXU%2BXaQJJOQnmrvSnfJ7aAEFZ0HszKL8rr9BN0%2FrlN4JkeVrCQ0O2fidVwqpawZkCSQ2PhnqIzeMmDmlCGqEpMHmIGUK8u%2BgmLckcsopXinYwEaZmQl4FgM4SY8KkGPHFwJ4tBBz5B7ROHUeDiXSIKRk7PPhZTNASAEXZMJu%2BAaWK%2FKPEW7z%2BOcFJli%2BEIB9xJkHwfJOLqPcwe%2ByJMpabx%2FDDb%2BbbHBjqkAZVf9qy1kNRLg2reuJiy3ywTNYtDC8VEjy8nj45fVyWIkTJwJI%2BXxqKWZiesH0woI1orsgNWU%2Fd4JpcMUcMzfFL26dTmvff0983ARPPIaiqUgIQppaeXD%2FgD8pQElClsoxoutV2YfF0ZxUN7cZ6ok6yG3r6KlhLlsaWPbJn%2FM4REdgwIgV5akvmBODFow6iejIdakFUywTNMl2tc%2FvbvwZvhTDp4&X-Amz-Signature=3ec960f37d0f9b256554e434d4745f22c5d59e5ef6a068bc0198e58bb8c36ec3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

