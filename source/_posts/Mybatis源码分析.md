---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667TBHOQJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIB%2F2mNWTj38EaVSjKbdvPqIbY18qLQirr9%2B427U7DILgAiBiihVJqBqAT7G1ZieRKfwPyYnvJgksze2Ax%2Be7fp1kdyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXFWQ%2FQjqGh9VnFiNKtwDFEvmJ3Rxaw013suhta79DOOC1IXkaLpNId4AaYa9jxY4jiZXYWQbQLRztZGSYX7vgWX4BY5eZj%2FW65xX7sYzM8SXA6XUG8OGKyCbnFArpiMKsWmZ5CoflJ6i27t1lNGmoTsoe70Sab%2BpUc52FEaSkL7IXXWvf3AZN5Da2dHfEcta60kMWzr00vRO2bFBrbhZEeK8nxywPJsGPtJmTHeXZyPqFYztHVtay47bHuNbkXTZGvANZoAXwiwlgcHZAxZkjKL5aJkrggAZTsAGpBQE%2BDtXX8WfjLAWDdHxBhk6BJWXKMHWdTZKyqk9GypLboFXO3VSslro3ptB%2FRO0qOutV2fOtxuWLqp1Xlhc6hxNd6gUSB12Tq8uFgGhkHTr%2FcVgOZF2AVNd7arKqTO4Oj3vg1Sau4dc8TS3ER6kEmzn2XBLReYHUu%2B2aCwTjvV3EIkgNC1UvTO2P8%2FXKJfV2yb%2BkBQWbjo3YVXFvWm%2Fwn%2F5nGoTtn3BFqocHs%2BCuRiv32i1%2FFWGCHNo2UEM%2BnEBerj7Ev7nxaHh%2F5IWlPhuMdf9edXoclXJD7fI8gza29fOQNfFhGnKFZKVMeCisVeD%2B%2FtOn8lEqcOaMhKz4kCuevySaHUtqoIbzBswYP63Nv8wsofOxwY6pgGuUIJnnl43lAJDrJl%2Ff5CB2I%2FeTtdczNRFcrsKwxRmxW2qpiyluSKqDNwXcckAYBFLr5BUeRuLebj9SBG%2FNhVqscjiqr1JWk7v5cDpsI81r4f424M2p%2FzYN0Zd9v0WOUgx5AS1fjbgEUlqV2fSxTkAWPzRglwvn8XkonTTBwqX3zdPIuErwRBmojpW0aB2f%2F94XlbJwIs33g8rvTiihE4tRFimbQNQ&X-Amz-Signature=14a4eaae2f073ca805689ac5a5f3ab6c674a57b27fdde19120a38909b675a21e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

