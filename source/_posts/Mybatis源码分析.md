---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZOXS36%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T130118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDIYiOi8gA4oKBcc2y%2FjLvtw%2F0TifxUjiTLUfQP05k1yAiAJKiBy5jFm2WAhrJKzSo0e2S2z3xafrf8vA4A8ZZ8hISr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMfb6p%2FpDNCYwHiyWrKtwDTpJ4PwDK21NNbtcuFL6I9iCdKoakLpgQpbTFs5icrSWtNTaV0EvfYdRtWeDefVsbFcbMle4awynPGc4xhRZ2b0THYe4uIyn%2BguHhYI%2FdmpU6TFPkB2xQN0f%2BGMhNxEd%2FVbCZ5xqNBJgBorgNS6SFmJNzAoJyj2AlXefqd7TGj5r5%2BR78qrEemhPPaq02Dc%2BwhTowRK6JRqnrwrGal%2BsjwFnRs4wf7n65DMHb9mTNWVhJw7KtT3xZsfYoZd1TeyPvsineBXcSlasvdCswOvEfVItJpZXfXmm%2Bz6ovKW9%2BMluh8R%2FCozD5dnx8V7jwd9Lr61Fl%2B16OVpij%2BVVzrr1gItM2G500FYQHTHtEl6WBPYmqzu7EYWKYGk1xA4J0jLaWcrtZGm3EVKHuWNJRZDD5YbbuKpnP3P4ln5SpFcIFZsRINc2esx4zXbFbG1cZTuF%2FdMQ1BH3xRnEF0n2l%2FZ6EqUfFri5VA5Lxgl%2Fqp%2FVOrRoDVhVeG1t2xRjechQIazB5fSgzHfcKFdgkaKmBnRxBA3EkuYHw8KyJU73yfosvU4wd%2BxFJF%2F%2F2vGTWr2T%2Fyq0gYIpDtHC8hknWVhAXZKk4iuysdc%2BLW2ymuYHXjvNV7kMQxqatMUKMmzzR%2BJwww%2BCDxwY6pgEfzW4bbgS55QlSXgpPZqKGCqhpAKnSujSfUqhxhG0wrhtFwAsaFq5EDT9V94s1JKvSxBlJ7CFIocW12lhG0qeSmq84NqPy%2Bh4zfBAI375B2KrqYS68ZceJ%2FXe%2Bi1HyJSRYis6tIjrVY6fJpbIbLBi%2FrhzEXgselPxdOIHTT0OWBLs7ZRWjEsXhI7VO6leqqCsj92vI91phpyVXi7O7R2QRQYisJasU&X-Amz-Signature=a492c6b9cbf3822e4bd89abfaed861bb97f08fffc385b66fe97128df5cdad9d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

