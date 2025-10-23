---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UFVQNHS%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2F659yUFvdJyjXsXj3wnO0WMkaf2rrRLS7j32bKUxGqgIhAJiEw3u4fCsnPTQStyYupIxrOyVxk%2BXrx6wsOXGl3X20Kv8DCEgQABoMNjM3NDIzMTgzODA1IgxLzSwJSIYvz39sjukq3AM3hG3WcnK8v9Ib%2FImsOiGVANPdhI%2B6ecF6AqaUS1vAYMAzjf%2Bnu7E%2F%2FnskrJd4gCDzUhmeY8vCHOHYVITVtAKja%2Fq%2FpYLsP2pccrYwj6xj%2F55YmSUpIaa42wAMMnOW3lsjYT399N64FZ9oaaxULbbEZ2rcH6rj7UjZGRbl4YgWlhczQgN8I5gYAYqckyWAdccts18GJybyRSCZcoYWctJo3i5S4VR%2BSMdpCDLVXSYbJP8oWVykZh8C%2Bu1NkQgjYgXzjxyxwHdbBq8Y%2Bm2uqB68dSWeFD5BCtFSDNrahYV6isvtb%2BGXxIue898gFDXj2ZsujfOeU0QEQVknpShNzTNK0hlOmi5ERgy0aPMRIQNG9is45nloJ%2BuOmmXmkOIgvkWcyuV1Y2SaSmm7p6qfKJP0op4mogIXO4aBQDZE99WAMmKA0sNnqRDpIYoRPTBZQuL5h3gOMd0Kur4XUH44TDv3QKRCpGtppbbEgUkdNhgubvL7Hun4CTF%2FAtryQGK3BQsDQB2DKQchjouvoBfuDUyjpySaEwkjXNMML2bK3%2FVo3Nt2QhoYuvjH%2Ficeq1ud7riLATMZf1KWzKKdWmQDMd5BOfsjx%2F8PCgEnFe5Tid%2F7uNeDzZDAgzSDSYb3QDDs%2BujHBjqkAUezjWVoG58R%2FHxhnnk6vXd4Mn2X8EGBTB3LWzl3KZky4tio%2FiDiLLTIKKAWB3krs590M4JRt31kQTsp6n4FwB7VT1ZwpfYPU3pGPTHDcNwtsGscHlkF%2FuGMsD3fkK48CRN3NHm00ZFJ3qiYQ%2F3NYEbZoX9b%2FIwXK6XqUF19cKyqMvgErC9ll78xnKMd%2BGJbc2%2F8sWLSLdF8OnU9%2BaQjwXbAdueo&X-Amz-Signature=da8f066cf66b2b9c5cce4b8f3da61bacedaf99fa69292cdc25aa62fffece2b7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

