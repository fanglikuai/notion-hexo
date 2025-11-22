---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4FGLX63%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIDyQ%2BOJbYWAQoOu5a9mrSu9111cGefZDiuV4p3t4AHDgAiA7G8k6pbhFcfvjDgBdM8Gcztwhc9cGSZ33jZnDvjFEnSr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMCeYvbbSHd%2FbixWfUKtwDb9EwimqbuoBc2vS1WcCBQDfCD7kWgx%2By3xZThnBjsmT59a2YU0v9yo1NYik0VG6C9sz8c%2BwTtlLXWcpyJv91cCtK31iZbjXracUJUnbLYg57UeLRqA%2BxbSXBigTO7QZhG8wcdq2CQ2ca889U1AECreyYW9fYq5Dl5uhzxP6JKtLuxgv5yzNvmbeipabDMnQEeMUuukQMJ4%2BKtlXaXy5%2FKGp0OQWBtV7dRbtfsMCIFFpPS8yF1aXETWH8mFYDPuNbMSRgOdTO%2F6R%2FLz4QaF7hZJ3vbezaXXFgjYTAfGqZI1wu6X2BnQ0TXBruotQ3pvBXbHNsoE7VwUqNGQW9WX0ebq%2FvktJyS9N8TgX%2FaA414fC35aJ96xrjFLFhQdMK9nsmeCrTA5UtbVSvqUm4rBhyCkMYGcg%2FbV3jiBdoxbjSLzPLWKzjLdCxwRETBGuMqYVqnqvgF2PQiWlzdNhaTA4kYSn8PJn67OexrFH1zr8No%2FwjvBSi0P43D1je7Yu0NWopYQn9FORlSxSjwfGJZ8C4TU56W4rkbdxEq5ELYx2lm0JsJMqi3%2FdFuzvdGhSphQZJOgxDOUFv75JTDELcZvw2lCK3YhEEmJuC0ytJX%2FphXLM4TC48m%2BxI0q6eGzUwzYCIyQY6pgEPUhjLtK1A2e%2BCj40HWelIbf1xz8l4fjkjBnBHeeKLenJLOlpAxLd0VxuZ3AYkEVvEmGnF3WFhr0JQ1f9ZGMLxuBBr8FRjGswSLS%2F72XJHNhLCCXFX0gKvqMbp3dw5obJREsT2KrNy7vppiyOU2IJMolRP48GUgQtkozUAVXXuP1bqTsyz%2Fk2AUOpCPEF7k9KYI%2BD0k8MFlHGIjLk46EVE%2BWzxe1cu&X-Amz-Signature=99eedd1be24ccfb197816ac4de7d7a698fb29e0ef4ae30cdcd2e638f87e74e21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

