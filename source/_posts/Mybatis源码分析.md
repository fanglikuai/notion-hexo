---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7CGMXL4%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCACM0Brxlt7fng%2Fn185Oroxhr%2B%2FBlDjzvlt36w7FxBFQIhAOy0VwPx3JZ3EMp8VVzLcwFF8KvCGm3mwcDfnYsKryLnKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxYT6guqVF7hupMP1Iq3ANiHebzvkrYzDC%2FBw9RQLH4nN4t1mr6M7PDPD5eCQQz2iJaQbFdQO3dO2RDzYKzyKHXgOxABzAU0vyIggk771yoFpdshUm2L%2FfZxfo%2B1wBJprwFSYsnK1YEahdpnyFO2LaKSEkAWATHReP2%2FhiwsUgxk6bk9aXidRty3KTC4si%2B242tjAT9QEAIn%2FxKU2c4liJHHgZPtHY%2BxQyf2WGju7UZKvYBq1FAXEXV99Qr%2Bi%2F4wcoYYQhucLToLeVXp2GEFoGqfbMmHUgfP7GI08WVML%2F0SqWQ0WLiKShff0RZpq3RbPWFkre%2BKnUcEDcUO8pzlGu%2FAa%2BKdQgcc54FqkqtfW%2B1mk7Tx1LG9UoxJl8%2BWq4dtSQSF1cGvkjoWY8SXuuxdFHVMifKIO7Zs4iT%2FaJBJiv3cEqjhJYTTQHjdUQsUd7Sp5cCI4Vy4MyvM4pjC984Opz5KMK%2FJ3%2BFrdgeW%2ByRmN4lJKdLXCIQNRIIzulPvlQKg0s54OaxkOV4e0yVR2LUENNcsIF4AaH5JlwW5b%2B2X%2BRwqg3GJbsJdtEF9QwFLnkUmoOWpyrsj%2BhUGdDxND3zB3tPnYAqeUOXzr%2BQrcj3i9%2BSDMd%2FjXDM%2FpzudGy2GgU6xoCLi8BCLW4ByvIEhzC34t7GBjqkAU96sHdtrK6U19O4TP9cy8%2BpRxxKO51jipnbDg1JD00eB4ZoBLTp55GXQnuvpaD9c%2FCp9R8drbqoQcnx7dtCSDG5BhJyDQ%2BnNuGKmDW5fdJ67aLzkj07wJN5QFeeDbNKOZ%2FH602fKLouIVtVK%2BpyczzEICXEhwKjvA9pM1gG5FQlBqw2mX8IqSZ7jOWADYfAmgKqiEiYlwWcnNbtJfzrVMs7Im8e&X-Amz-Signature=50ae89cf0ea3d9b493f9184c40ae453e6c911dfc38034f3a6f8ac4cd63432169&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

