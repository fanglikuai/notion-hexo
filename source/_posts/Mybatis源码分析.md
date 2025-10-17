---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVDNGVDZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6m3zhQMsSA4FynjWqi2jJCXBBDpUPcTdTTw1WRbR47AIhANIO4xsRQBLzIHGc4J4uzrrtShV%2FPch1AULVsCoLGUMDKogECKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOaSrq04AJDdy85VUq3AOoDTOZwbRtJv16KJNLIS0lid14yOqoji9vBzysXBD9udwLM64WZs1Vs%2FTo%2Flo2GRfjS27OfKvw%2BQVxysxsI8VbGMXlAeg9DA2YPJvO9giqjFXj2OlDCcqtl5%2Fg0V0XBDMJtS44ljnWX0URUCKTS8%2FDE08e9Ff4gFZXAKFKzDvHvWkB3rb3ETLwzATzpfejH8vbQfj%2B61DkkwcUid1j8i1AVpNEy18FGvkvycXg9QhSIjUCQJoF645YG%2BVqjp%2BG%2BTdZgh9ttmpRPst6eup9ica%2BuEk9TN9voHpQuximRwZLxeCg5JSUTaLlUHLDyzFzlDTTW08GngXXWJ6QXJHv6PYojEfvnMhrSmLGxZe1%2B7NwK2KkqFcC0QtzXzKBnBr1Sv5S5LIhdwFbqpQjoz91SNPfHNGfDWICOK7Rvn4ncPFfmLf%2BE5nUK4t%2BYN93uIWUo3Ql%2FcVtQivHxgUQB1rb4WgIkltYvARqqfvfIXzgkpIP2MnwcqnWCBrPxCkz6T%2BklRV52K%2F8R6cPTSi%2Fmxkb4Tj1jwAOGczjdLv%2Bz7AsUcu789XNQl2AFaXrdWR5vRJaXawihryDzRuq%2F4JTbFDcLTjBvZfy0NY5FyO7ar6sLGr4mtwn4UDlAiTl4p%2B39DCt88fHBjqkAfqgv5PUHe73ssob69l4xEl3ZATvzhoiCtazkFPq6aatEmDID6Uav6gFUXR4Zf1rYw0XueA2qf9bdO6FpGniRVTCjRPkQq2ITESbvGTkrVC69OgWCHsmZXdkrFKYSJthWL0U1TXbyt6TZundteHjIS9aGLHOfIgayF5ZEeCNsRa7WpPTpDEri7kMWKlyNy6WAthYWOQbwpqq8OQi3fqmjLvZdjWg&X-Amz-Signature=cb51c519f7e03a170e8897bb9a3c23340c53f4b704dedfd8b080b9dffb1726c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

