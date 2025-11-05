---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE6PVUJR%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAcyYsRsIfYpby%2B6op4UMemLFXlSu3L3cFZczxwCO6bZAiBcd7zAyixGoQE5Xb2s4TQZn4c9cR7jwam26yDAls2yDiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSpXDirPlyyjOlZGEKtwDSomERcEg5075cMKBmUWD6JapVZWaOieXk7OYQW8wpvs65H7xvToz6vvWrLwtcSrcE%2Bme1cAcEhjNfPhkVsgDaSY4hYKJYiHLs06dQzqAvg38y5uUrOXHI%2FjJTjgwn1W%2FlGvTaWnAzOPtzKKKHcAfmReldXvw1F5omB0yTo97fDcNwvze2TeIW%2BPF1xXMf4ArCL0zBxnk9E2744MgeRHIy2%2BPGZhFCO2aIcRZ1ZzuvcEeHl3VtlhyyLchGgNkqPs5qcuvnWwvoZf8WuXQ7QrDUPoStPrfzOk1x5UqPuYryU6xsCVFqhUiMj8Q1l4AAoF%2BdhohKildnhmT4SZZNSAbuRXlTlHjcJUT8sObYBU84FhtxwUCdVFsSu%2FzpKtkvqUppTTT4zqWy5Smxhlr7wAcxQaombnT%2Bh%2BhHaCQ5H49UEx%2FNqO4Wwz6n5HBAoQ6Oy8nPN%2BOMZMbRrfqzxqZ6uYQdg4ZXOMGCT0ZxzLgmXFobHRV7loXVj0UVOi%2FtihaA5Ka5cLzbHC3aSO9ZczZQhoBK1FwFsyeSEuILL%2BGFceWKq4lEPhVFEMGGHgL84QqLhjpH5NsCtwGP6p5pS12m7cH3X%2BI4LqPnMuMg2KB3wQJgzMx5vGyjdOs%2BiCg8kMwgs6syAY6pgEKRgT%2BVA5Fz%2BhmVnxZQXXgi9g%2BByCro%2BQfak%2F%2Fx%2Bsu5MDTeqkEcWJRQxcj3utX4a8CfqwHtmaYP2HgnDYcDmQ2jGL31mILrUsC0A1JHKnIaWF4XA%2BOglc0xaHZMm43W3C8gosvjpIQKia6fKlKtMSZt5SemdabMSEpoOZkiCel%2BKHlusgulgG1jRq%2F6SrPCPJYQeRi6kVXqg451HWxb2j8pfW3Q5S8&X-Amz-Signature=fd4debdcf652469f4f08a7b7a7dca840690f8a4e56383c182d2a6d921c71fa4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

