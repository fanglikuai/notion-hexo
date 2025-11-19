---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KSBXMOL%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDIYQXtSg%2Bi7I0%2FDdUjzZ5UsY%2BkX3%2Fdxnpv2HSoOP2sggIgM83LpJluZEnVVZO2a2uHVvrXtxF9hlxQgqVHH1%2FWeQwqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOxqhFZLhmOUHmilircA4qbCLVx5Ne5RNDCijyKtI6ybM%2FcIwm0kWb874z7G8ByRdVA75R2lojcqQ5gB8fyp3%2B1jeaDh770em2YOHUExXkAG%2BcV8LMsDiWvh2qHjXqfGYhYeI1RScrOBGofdAn8TAy1OsmbJCC3P19Nj9PPPRd29c%2BLmWmKFDwb6HcQmgDFHjkr8jMwj74Vp9Mz8vtgDOTLd8q%2BFiCIJ892W4LP8j%2FVOzRB%2B%2FIHUJHjQow68fkUN9EX44zTiSYJgTas5i8IhXS%2F5pEy8%2BTXM4PpFgOFqBntg2JBYJL3z6LprmZS8WMMaMY2VhwmfNW6BMbsCLAp8Fh7VYSfzsKUVLdyXSporelGyC4Nc5kdKPd9SOCXDzAP0TlNjL8unmjNKAw8aIQHsZntFmlnUxNO37u5YHQGbLb1hqKWPeuUFU4MUsHahyXgq9WArQVIA%2Bp3Ffcdej%2BcCrAgZMtaxlZY8x0BAQIB1J1Sud3P5fA1uGfKPoM2%2FQZmKng59EaPhNC8lNkpcyUVwVHtP%2Bz%2BKJZkUtoJui4c%2FJyBST3iAAhHbE%2Fd7r0s4P5%2FmDGxImJfeVWyJiDlczS9gqYxF8lh23SLdUi4pX6uohbfz7ApbvZD768VX3i%2B3k6PNRHaFOPQzSKQvnL2MPaV9cgGOqUBsgCVYjKDH2i1FhiMmdKqQXyftKo%2BICQhcc3SffeXzdVAL5WOzusye8SPHEeO%2BUxV4xAByfiRmx%2Bbe8xFCAekakEdJS5jJeMJWRKXOlhjTfFXtUUm2yRDxNX3VrRYZAEyGDzE1ypYG1IzSPUK3MiSbAhvJ349Tabsl8OlVd1PDj0sosmy0dgyosquJHF3I2lSYTUKLpvEZWQaB%2FKgGY2qJxTBa%2BBR&X-Amz-Signature=68d0059d51a373bb00297749605b1a1c7b585c9b6be552ddc90a3d08efb78659&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

