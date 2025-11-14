---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YMBBQTO%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDokX1KLwBIJrEFf3E0Zm3mw1P4XBkt5abX0xakiHGjtAiA%2BicDvvnAd0uiuuvcjkGDwHT6izK3aJpzvolv5we2Q9yr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMdmWT1hoNevaWbf5BKtwDSTeei0f3nhrBmOv4j16EBIwJ%2B2Q3q%2FS0%2Fen6E46bPJdHmrxQqgzS7xZTkDizFYnuA4gnv06Bgf6aSntTqGxuC3VDeEXR%2BWOLqx7Qt%2BSYBvRU005r98eYXeC8LPRDTELoV8g96jUJLs88mgmkjS15VJkAf85bTBhMZuh7ODMlyvICgBR9XcPpOmoNGxIhMlao84u4kSwivQPl4nxTZGZcam5ndS2Fp8UhjgmPf5OZ%2FfgZNITta61lm0%2Bnvv4sPqdfhWEBm7HykN8%2F4WnK9uAoXQysph7I9q3KksCh0Eo9818FzXqKbcFnwzj4U6gGEjYY3OOQZpYZopi0ZCLwUZJIWTBHNAq3sniEneo0WY%2FY3YgdIrm0CT1Ur3u8EIMR5esY51syTBxujQ54Xmey9jaC1VvOYcwUYqwcTWHz8zAUtZ0cmxJLqHwBVomUdRg8%2B0LglNJuAOkipnMp%2FWAA9edcv1CBxMNBO%2FwZunhRPO04I663k5XQ%2BMdezZyplWuvA6d3QD73L8r4xHw%2F53WorjQUpSKw8vZf3QF95Wq67Qji9Dv6pclGGp%2B13s61qEUMDCP1R3drRJ3Xp%2BOKfFKHyTZQz1%2FIex72PNotWDQgY0CxnkI7pafN6P7aUVktrbIw1ZDdyAY6pgG7DhReogXs8tkmrvwarC%2Bm0hzH0Kr0Y6Z6%2FsvixXXal1Kr9hQL9O1fPZv5%2FhvnDK3CJ93jyWshGgeIuqGys6nNm4HXj1%2FxApaMrS902Lx%2B9NPyqDytxWNI95cCxtEGS2xlfv%2B%2F2V59bqOe5ofTns9Ck5b06KHaU9SS5GsgtQKiOduP%2BBJfqcldR0ohWJQZdGK%2Fc%2BqbYM4bs%2BOAkixRz8HocTobJQea&X-Amz-Signature=b3e57ac1a529b215d0d9a96b38bcc8beb9658597121c69274acbec593c4cd35b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

