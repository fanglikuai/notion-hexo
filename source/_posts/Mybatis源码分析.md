---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QTKHNHR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCGl6CAQl2cpQtCti9H5maTFvFE0mN8dd4khHFkNk1I2QIhAL1F8%2Fog6CJxKYMAXVvjn2UyONy4xc19HzsFvF%2BGIdBqKogECNr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igylj7jNfsByed9KgHEq3APXRa5FlsVsEd2e2ttyYVLfdcMXLd19e66F5pXV2sG5eMk7dFf6wH39sbkk3IwT25mo9uwyeZklGuBsYl4LY82Vj%2BwVc%2BlmDcD1aynlEoiUakAYWcgM%2BobHEEBw9caVqEfVo5iy%2BaVC3ujIH7VG34TOVj6fRGgtUL8Kfnhy5hVn%2Bov7iTN%2FgSiHCd9msS472HAZdXp87ZfiGZMNeTEmz73IW0CwEc%2FRcFvYPPg5uZvW890Fqv2JDMNfcjACL%2FtYnR%2FjxoE1oP5UAyjo2j%2BlI2BWJwl9k%2FiQroxbcmwzZVH36FNcXPriYJvZ%2Bb0rlh9ZiLQjAO5gdXLqfxgfCqCtTKuCMbcRM4uq5VI0BBKgQvat01uQRR84xaikW%2B%2BNd1xdqVTh%2BgdoW3O7jLkmRmUyuxMxvaauKZFZpWgU6drhy4KL%2Fb06h1XvGXw2glo87j0wOp9cDq4NxliYt9%2BovuE8Iq1sQL2Eu5fFcI2dDjDxGvJ2zrSK8hVUUf1pAB8rK7xOBfCdQeyxJdmhKZK55s9LMD7%2BKeUenVKijOtYJBbWxY65T8Rt53MFA5sgTa2iN%2F%2B%2B8x2c32Ki1mI8QzEB%2Bt%2BRmCMkvaALXr%2Boyd48YCJqS%2FREziHjZeTl2i6Z1YdIuDCrnInIBjqkAQbVCLCv93lTa3v3e59kCmFQrNWl5hfZUJ%2B615qWJH7eZE%2BLqm8I2MBYQmr3Ht5%2FCTMS2P5D%2B%2FxSUyGbn9BVAU7lX2nmzIJ5ELS%2F4r07rlAPMhskMR0EI9hXvx%2F1XICndwndoYepvXx2URhlj4GbjMTAgCwxvuXlrDHJmyQCim3qvGk1tPV1AWpGNp565GQAA5EgBn1pfn%2F5VtF6BQoAy9FIvKuh&X-Amz-Signature=4a0edc47d6df8fcd3c557e6ef65f77e67c852106a21d232df15163d41872b496&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

