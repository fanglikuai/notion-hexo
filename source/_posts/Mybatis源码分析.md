---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRHIBART%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIOEvEByZT%2F2WbDADEh1pnrtzo%2BtuPzrUQJ5PkqHrP2AiBfkMOUiyAFGIvSxZ1XXtsiZsAJ0HFcXm8C9JAHj%2FfkhCqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv5y%2BGy1ru6T78bk9KtwD6Gbg%2FjykGXroOXyeINcy0mLliHYhdKx8q%2Bxt4EpS0xmYqq6Qdq5bAm9GmRGgfWySD9IJUjHAgyFcaFubmv6SnCL%2F6Zh8cgRJ7ld%2F%2Fxz%2BfX3WrdtXZ%2FAkKcISfiFLi9rMWohmI54tPk6tDTIzslOBxQSksvS3%2BA51QGa%2BWEwO8IGTAuAY3va7hIyk7cjjWWgkNnEaBQUU4Az5nxfY0Z3hNgRFQ9oqX82yD9TGFsWBvChQBdBgEuBRqW5lTycAHnN3p9YuLp1T0bVAJGbyiuhz6wBJ6tudNBh8brhLAeygy2iTsUTr8jDh0lmB3RVie%2Bnv6Oj0bv3g78mEbYzorn77B6u6uKZ8JxkqOTE2rpcLiStlmxaVOjBUpgWhPVrbqlzjXmM0FFSRwZ1E3NYHvknXFL%2FlCCHzuq3Z5WSYZWho0bIJoHkrKjxPt1T4JLM2OtUVxEPDvmw1AWQtVacf53SodNuw79eSGMm6Bnw5VswakB7q6k9xHejP004mwKtqUpThEgXHioM311rFJ9vZOillk8WpKPF9mWgsGCRL7xV0826kThIpNfuclVv%2FcSNqhXpZ3r4CqEX%2BoJNMOOHbqNZbOxKxRrcKua6YZW3jw7sYoTmeBpjBtRIndCbFVmswgp7BxwY6pgFAh79SSgED5GXoEOe06%2BO6OE5p64iSsk06PcqHNOdfeghMIoS264NPL9hAR8%2B8sJCHBkc35cU8cHZi3wYf6KZsep3mo1hEHAdQHgJ2Lt4F%2F3zLIg0jp8N8ZjqohHR4%2BPcTPraJnOw3sdONC%2F0%2B2EJ9qQZ6E4pLkougFuiI%2BqgGNtt%2B%2BuAHkdYpyk0orNcYJJi%2F1O%2FvSM4vLfWdYYlsO2L3AvKX6oud&X-Amz-Signature=a1a6fd1b2931a4db548a9f35b70404cbbd30a7eed6635e6c2d51fcc2c5ced079&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

