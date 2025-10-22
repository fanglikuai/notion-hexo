---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6V2MB4X%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQD74PDqxyw1Wqcu%2BJS6TJZxXOCdxo6PdJjtosicj6KnfwIhAJpv6Y9IzQei1tZu%2BHKjnf74wJQ0Rt%2FT1BBCxZohQTwKKv8DCCMQABoMNjM3NDIzMTgzODA1IgwwAi6dUdnc%2BShoKKkq3AP1lM7OddZGKI15M6yNJvuuMnOEZTpz7Q5VB%2BEG96yqAYNWtIQdXmsg8og8QuGwIEI5fR0Im0Cczrjx0%2BN0aVoNrTXPuVYU6rnUiw2%2B7FR33eDD%2BTOhnsIgxt9wd0xZTICaqWgCR0LmM4mTHv6r2HAUPZjazKLusJkUwywEM0rZ14WYhYN7rhwMGIR6ken7l7XAnEQF%2BPpc2thTuXbD%2FcrLXPzQTgBH59RPirExcVqWROMdXXnNeBcHqtoF9Z1tbAT61NAC64yseYVrLGqssJC07XL3zJxK%2B3rrGt7iAJBzhRZjMKFhAbBJMvXs%2BKHla3EaxdtspJK%2FusPJekRriHeCHLbgWl8StKTWi3Tcz3fJo6AYgXI%2BC4nRQz3yePGrcZbOTe5gCnEhNpIjkwsh5T3f5zXO1qzVGzlIoc9FE%2FPwAyYfAdmq0%2FjCGvJLbLOmZ2ESvWLU1py6sCWN1EkRFhjuFDw%2BsdjOKozPgrJCb7FVeihKjgFJNrIhImsxIfQKqGVh1KvWdMbfqV%2BGS6xRI%2FRmzkEMeMLZjzFi0JtbHw%2Fm%2FGK1tVGPoSculGHTHmbHmBa2kAo9pwEGgi5fpd%2F4jGihrUyKwRkNKbfa078F1as3R7llN4lt57%2FpNmwLTzDw6ODHBjqkAUcWVV9skk8vEq%2BPWe8OtT0zTqedb0ITXo5dh64%2Bphc7YS9DpoTvkpBTyl6hNbTGeYWIPtzwNPSp32twn2xzO1gO8DzeSUzy%2BSnPAVPBYfM5VJ10blYVixBL%2FRlcgnqKXgImXeC%2FtUbL6P1p7MumsHgrBH%2B7mOkP56TfTONsvSb3IcViBN1R%2BEZ8C5YzPE7dJtJXgJHfOlbrVqxuVu86%2B49Q62pB&X-Amz-Signature=29b9396783cdf575aa06aa1934a1e1350d8951b6703776f66193c6c155c91df1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

