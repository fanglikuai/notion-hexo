---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHASA4HN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIF69D%2BYfvbp6DplB9Q6JyiQNpvjA3YEL2DkYiJpasOO1AiBXfH%2Fk1ZAW%2BGCwqzqOAyPW%2F0FPZhp2UOPvw2SpfodrOSqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeR4YkxBk7TLmr2nAKtwDO%2BU4an353OPD2uzZEjQBWgBeMppiVw9sPDMKKcR2RUuSCQD5Iyk1gjeSbibdhU9mY%2FkokWBdl3tmEur0BxkQaJREDfVWSsw8uc1ez%2F8BchMOy5ZLG%2BC2LdcWqresSQJ6OIvxPHfmg2KNeqDXtCbgYdi1XPVHtkjowZaRYwpY98jUD%2BncxBWN5eT13td%2BmVlEMm0kMPMNNze%2B%2BW8pX8BXmvU7Xn4uQlhOsZxt4wae7lB4jIRWBFkwG7M8tFYXWQeG%2FkTPRd%2Bj%2F5AkpoC7a%2BpcGAsTry6uYjR1CAsi8NJ5R88DZVZq%2FkWzW67YAXOolaX6vL3FPR3Q8VeNhsVCM%2Fi7fceABGjQGMKX6RKZ6DSmAC868%2F%2FaJkYWLyf%2Fi%2BGb%2BAVmyPICj4zqUVmu%2BF0Lp4%2FkU1hywJpvdnS9BOiR%2BpZ9v7OXC%2B4KHgJAKtUzf4GD0cCO1clwtvMrC0fXoveI5lFeLCNX12c2U3edkA5BvUQ8p%2F9eUJyNqqaIl5ck98Go9B8Uzr1VvVns1CpuBFfSKB3D8PO5PeTlNEUvifaK2fsXkEsaZ9rVW6ATpWTGNIQ7aLxpEINvSyLc7kWzVt0V0OmysT6zBd7%2FQA1leX6%2BqU3cFSiR7WnNq1PuV08eJ3Uw%2BZjUxwY6pgH0KCPx5u3qNCQEkoypay00jhESdjjfFlfzMcCLLMMeqnbawhUrGd9XFDsjz%2Bvs5QmcEPbavz40BqolLvwyL34thhfWUqC%2F0cUKDKC1PwdoXHwdBVomeqAMLn3%2Fzb%2F1ID99Jp4sXbjnRm%2Be0K9N5jSSGve%2FfwmjmksOCodlr0tupRaXyWpn%2Fb9bqd8j64vwRn1h2Gp15PgApe9%2BF9UwQQvIs0ciOghm&X-Amz-Signature=d35183f24a519089404f721c28463c65dcff412a9428dd381bb5179278803ad3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

