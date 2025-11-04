---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PASSHC2%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGaL12%2FQ0Eepd0nNK8FsxAiBJTRWPZ6JEfxHmRqP1aT6AiEA6XVUlbHym0vwXxDBXisSuJFYHnp2WS1SNopw1WNuywAq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDKCsujg035IWROuSiSrcAwr12%2FaeVx%2FaR%2BEp1xuSdctW3vej7pwX72O3bXkaPqzhvHAXpnB3e05berOftSzqP9AiLbigB67x8DV%2BVg198XBtWIks%2BwkKhR2QpxRJuf0ZV7MawdP5XM9P9qdkHNOmEc26L%2FAtfYkQOW0zPFSwmMSRL57jGVDmLn1Im4wbSsP5dtH8eZCZqObhvLGE0u8uLIdWT%2F3e7suDjquT7GiWVGJctO1xWKy5UDBBLr%2F%2Fy6PmK7ufZsr31izJYkjtri2CkVw0BEoBn3fUBMw5y5lfI75pQ1T15NpkHrF0D7LMBJRSKeJm2EnyVFfrAwUshqRGSDeFgb%2B0mOsXmmf0pBxtbIc9EX0qr3PgdIMTaNkyNyxy6IccigNYaKCaHEMZnRv68tGmKs6LJsPujOipu5xKeVkeLL9sGdzI8uSYKz59Uq%2BH5uILkKcLJ6mcUT%2F8UTsdm2Cihb0r8a%2F%2BGLx6qtvLU%2FtSadXvKuV8ZC1BJKflFFKM1Wakvd2vFUWAsW0fFNzFCN4hy6b5qwgR7Gtf%2BiMTZ8%2FkaoZBlZIZn%2BrkZaOthnBkZvhnKTOj6qX43s%2FtfUL2XYuLEL1QIjpE1XItywqkIkMGzF6mmI9hsAhgqOv84n0KMa85dbs1zn%2F3HherMMTCpcgGOqUB8KtVEr9j%2BivoXzhlfDrPJumTbSRSxpMPtpQRrRfrszxDVGDucr0kDKPqY%2FLoLBTR49pKR7Ilm68MR5xQ%2BueJqk7czJQC4XahS8BfRn9i7DNcTN36eUaBBfJeCVZo810ay0JG4u%2Bg7pGwT21Uy35nomtqb4Mp8%2B8cfWWMXyfEAk%2FdLbsfSbAE%2BvvNCDCxQRk2UA99wfUnF%2BF8uTsbsf4m%2Fd6iTxzr&X-Amz-Signature=f211e8618df7cc9872c5bca2d63f7d9fb2b997d8f77fece287752462c062768f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

