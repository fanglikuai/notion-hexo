---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABGOBTP%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJGMEQCID16CfREx8QZ4mOQVnZUYKqtKKOD4xn%2BmjbLBERRQR%2F9AiAY2bSNhnQc1jU90jPi2CLexvFq7bQjfENsTlgJb36L9yr%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM6yngeFXmbCCoyFDDKtwDYJX6wHgOvqgDdZE46LR9JrPmjfN4PN8rqLeRwzPv290o9Dy6y4Iv9xqf%2FgVX0kJaPNZ6RXt4R%2F6kpCfsJV2kOSNs2B6hDnbfqgRM9BY3dNbHCR1J6ZirWiUhQdQmCQFeQKs%2BUi6RYWiwfJHU%2FGnBTaR0aElz%2BLgJL69ceS9HVoiDhyu0eD6UDz4oRYD%2BrhSJsSZvfo61hI0%2Bj%2B0H60F7YhDQkEZvl4G6PmvIdU1wVxu8KtN%2BeVpOpz2AZhGSMl1niqfG9hl0z%2FgYUy3Ke3bEGPjUdejmJWt23ByEUALHf4CwIZE%2BPApNX%2F4CjJQSDBXdx7HKI73o21fc4qh7Cu3QC5HhNEmsNb7r0Ky6bJvqt%2BFGBTsaqa9CHEDwLSmiODiI2GFh5hfCV%2FJCWH87FlDlc%2FhX0gPbALo8OoLqob5wWNSocAifUZ30QIswkhHHcdwEM9yfsyhWv9jHa4luM5hl6fqFQqakd0SuO%2BRx%2F7pN8vr%2FuUVHcX0FAF1OD7Xr0pCKk87144wQhLA8J1XIS%2BEg%2FNkmdkpaRyDb7j95uf%2BbipLslwP5ebtZUH%2BNedY0Qe%2FettTYD9lM25isgjY23PSDda%2BPwWtnkdRGvf9kCiabhOpJVsVVxIuqfcfSPc0wnOL0xgY6pgHzRikk9vpaajYs0yqTYqZFnzmz4HhWK32lAdd%2FT9sr3h7GqtCbBUhBelm9h1%2B8ohM%2BX%2BZAl7UuJhfKjnRAbpk9WL5cfbDVEMNlDxhIK1lJeXzUcVWqyb9qw88QSuAc2KA7ZQfSsWcIKjM2cIw%2FY6y3adykUSueNoGVG9brArrp2KRHQtn0VUpCFv9kr%2Fc1H7c5vYcArqi7dVIs47Tf%2BQoGJHMSFy48&X-Amz-Signature=60d9224d4aa073a17876512ced2c8a686c5b95af64915e05b443ef0fc057eccf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

