---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIZ44FW2%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIzuTagkvvnO7LUb%2FCYMeY8GGj1iFfB4TILY0PaA8RSAiAwhlBrlgv2gdGxMx2tUZdaQQeUgNCXukU9e1WoG36kuCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMufnFe9wLQVYV5WEHKtwDVTPb4OyjpFXeyYvo632d8dlgoeKmFSTzHjrO9PjNucBGEsvjxOzxN5wcfFc9UvJU0RfHToIG681Qbth0aE5I6%2FVg7DTl3%2F7uiGR8Fr0DkMNDSrSxK00zUp%2FzmAqehsz3%2Fnks5%2BOZfQ2c08vJH%2BZ00jjBB16wxspAN2HOx5tfeVWSG7I4DvnIl6BcUe%2FM4T1XsL5ZKY%2BCcfhDJbogvBuk40MuLmzv68Ypt7HncrY9ASGQo%2B4PjPSLJwrD6EBArJ9fSUUbd%2Bf7Qsyx3kDoYQ36oeTxFO34I3br9aHCtgbw%2BS%2Bk54txTEQavGN4Kp%2F9xc68G0Yj3CpDaAn%2BH0pS4hfR4ZGBUJ6H8YlrJBlLtVEdnEGolTVlwfAHdDukUdqCup%2FBdEWXurHjCn9%2BgB8%2BjAjKwVwVsXHSgU4ULxnH%2Fz2fbXBxRXTIeAcIWujhiyEzb2k%2F5cEoVXUUyQsMU7Q0q0CEoYCiIUEWvOxt%2BgtS8B%2BYwZ6aQxIYQshgwpH0vW%2FmJdbPg0RJZZBkUWTuwuungHs4ozg%2F41fqxYC0EGxZoxhrvqEKcsi%2BVeq%2F09DAXb0U%2FyBcU%2FafKw40heQZPmWYtwrrMq9Q%2Bc%2BadmaNhkqdy08AAx6j8yAsbPYu1KRInjcwseiKxwY6pgHq%2BSp0ncrzpmSExKrrpEDI8w2t%2BjBJvF%2BPfuG2BhgClb%2F%2F6wT74spKXBP1toXhb0BOVEJK0Fj4%2BNf29LH%2FaDhEh9FxNVyhAFUs2JldZpl35Km1fyz2NJk3Pw7LDQ6hCA%2FURwdr8fjzis8CMCTSZdbnlN3j2sdVVtKWgvv74VdgeovfLHc7pqoQUw11UMZ9wc99p%2BYvNvLvyBduq4D40tp5hR5z6lqB&X-Amz-Signature=943e67ba95a1badffe087a71a829fb2e815749acaee97d376d3210c55c76bea2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

