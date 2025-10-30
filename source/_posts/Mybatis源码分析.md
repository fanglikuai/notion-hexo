---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YXX4RSS%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIAa359vvNe3%2By4bOPFtz8Qz7lcuyAaiUeR3GDtXpDr0YAiB87H%2BXWVSA2T6AexZqGths1BuhwbBemq64qHV81eXKOSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbWTxzmOEduKgVe9SKtwDR6R1g3GJsjrWfN2X5U%2BWV4QtePVkILBP7O7e3Q8dvFOsachfDDQsAsd%2BM1hbC7yJhdh6DVujK6ar8bAn6mU7Jl%2FAHQ5es5Y15v3kWzY%2BGdx0oFZZYx8itgl6B6ooLdr0V4SpM96m8pDLPtZ3nb1STN6GS2dLisGkU2u0R0UK0%2BXkBy4IyZHve9Ge0ScnuokhrboqGRjBIs%2FV6iQ389mmDxEug5MnK8zOk%2BmAu1RCjooVIRq74rCsUKLJKwbmgutrV0GQsx3WFHnhBMVW9qsatVUumrDisDJ6xk1p%2Bpw6AR74JIIArh%2F%2BqZ%2F3ds5eyPOzLmHHfQfsdds3oF%2F0Fcd5VJGw37wFrGc4DAH%2FQFjD31bq8zrfOvy7k1zFdFX%2BHQXXwyV%2F42OJWblcUesff1ABQ5gAUI9X6Rb%2FYm6FnarUjpOzDl8QQsKhPO1F4q6CEhZwcjyj3wzM%2FxuiDMPbsrGad6DEcrRvkqY38mtQzSjOCKFLnanHZeO3t1eWdEtV6Mlrp8itrtKvr70uH59MA02R99smI5lWrbdPyJe8gvYluFGBl%2Bjx8UdAIH1APmLCMn3o24zv9m2aNM%2Bozfi34buer2c07d48q0swJ8McMW13ta0g00tvXUe82fdrOdswoZyOyAY6pgH2rvu9M0CfHJacNTZAiHF1S0lLENhZxeY7fz%2BuQORltY8cnQ9Z6n8zuTM%2BNpmzL8qHyRvynUsxNqkm%2BirTE5q%2Fkf3yP8vWi5CSNenZYNs37bfd3FFsU9gOJ8YYh8MfiJ3AHjJloF1PFY1Kh2ZZr69Vuf92XjfBvyM9GftXMSkRwTCPvEvG5xOhdg2qeR5BOxI4mWVp1sJ7y8EuAD7JFTkD7y18%2Btmq&X-Amz-Signature=803c71d90d965a515e90048c477f48c48dacf05b403508d97e3ec431ed12c99b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

