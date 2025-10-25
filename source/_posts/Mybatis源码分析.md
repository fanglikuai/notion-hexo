---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665GRJB5ZT%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T100052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3FhPLgZGVo%2BVS5VxS1AEaxDeu%2FA6gz2I4%2FzhZ86GbiAIhAPZF8YYUIiEDsKrFy3lHAv01IGCYsEDvHPUJeNfDPCbJKv8DCHAQABoMNjM3NDIzMTgzODA1IgyHpI%2FZeOMzCX1W%2BsEq3AMGzPhOJMjK1bg1SgAyfE0Fv8sbKkGKs2iAB4BIGj36biLAxXqvz4LVKdyttwxFri7F3ymb6oYtu%2Fc3CCZNvsbZLOlkSyGbvs%2FFskI%2BwqamxuPYhF7g8pF6cS%2BRTQjclMAwZQ2VTZx9AGVWMbBOWOgRj%2Bu6Bzlceo7CsaCnGUILFyX%2Fu%2B2M1VBhmq52UYGlcMnw9oCIvd%2F9meXkhFZAHkGwP6TGQpLmjKTF1VNdixdRjgnC7cfQ3%2BbKAY6AuzJvetjcpsI38lkF7edsKMq1OQ29qztYQBK8%2BDdGYwAxVTC2Ks15Zz09RZssArwpgjhqtLyZ2xFMJeydWHk5Ls5K8dHEozxAJ7j%2BgGYfPQZI%2FAhjWRd4BblMe7BJ9%2FHFARYQQ9zEbkj0dCkvgSF2FGYgnfiBdV93DPrXzQJ3EpS2QEt72DguziSwfQbHMYtpcHA0ncUatZcFWD4KQJRVnGnkO2i0y7kqk0ff9QDdVFRCo%2FAOcBMOdZew9AJXbVKv%2FwTtJfHwMe0OT7gyv5QZvehBRcBdlRbmKKk87D8Grrzg%2FI7LeTqne0%2Bx88gpj61f1DTiJIo%2FKnXdABaEh3TnVkWXk3A5S0i0A5nvnqDah7XgfAJoZtf0dfqTsA%2Fx4k4eGTDn6vHHBjqkAdkfH1khIlFzmN7QCmjzwuxVCqNrpD14MWb5Tm1tuIGUdxd9gdaNA2B2VB2PCtwasuSjq6Z%2FssMdNblyQsBmeskG59UlNZQ7HmHk6Kt9YYxjk6yDbsQN3WssAKrSZL9PYGiQw5qNSqGJA5LRVwxVkm5fja2UbJFY8Te0AuAHizoafn8rVT3mfgLzla1qQfDjKwGO2D0RzlGFwnW17s8bVgiEYmrz&X-Amz-Signature=40c617955ebaaa3d76f3461a966765d17741140bea17037e60dab54063ebf8d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

