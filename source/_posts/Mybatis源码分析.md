---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XC6RNV5F%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGdVUFRZh%2FusPUUF7QaTHVfOzB%2FDyNtR4%2BwVDaZZ0wo0AiAtEcdpcsXw%2F4D9D%2BFnJDihIvwMASoPHF7wqlK2B4gJdyqIBAi4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyFmrAcTaNEd%2BModkKtwDkr%2B0XK1HAlumwfvycvvxadThFX3yFo2V2UKIl3kvqdAy8z6yxWQVEUajCix7jETPwyECr2XMOQu5TL92JYI8ddsX3IPW185%2BHlbY9B6BegaGMGAfXR69nNM%2FohFNoZLPw7UGW1xlWfxl9K0Mu%2BFKegFuBosrXlNtFw%2By%2BN1JMBAOvzFQXLSrrEpBQGKreE3ngrYhUuEPGqnUozYrhz3GsnGF%2FMTQam8nFOjBD3FgCSvlnmYOMmZEAD9gUX4al12UyVDuidIjBKVxIWDdj3oN%2BcoYHM7zdHjS%2Bb0JE%2FuGxLEPdX%2Fw2Lkx43zhCKai9NURxbDFzxZHSAOWjedU8s0DGmNmR8XW%2BzV41hiURZAKobqVZAtDbnwxKbmiuHoPdzPZEzhPhGfFI%2FXFNGbQ6TUHVrcqdg8JmH8ArByo9Ra8kBgOHdm3W0sUDXHd%2FHw9lXx8KXgwjlnYttSAD9%2BbwnhZ91XO0AYqdpMivdWvQouxNEZz9nh9c6tunZZUtHtAHM%2FkhxPK0yNL0NIGXHXTGkKtzZlmPSz3BHN5bdNxABFvD5VfYdjOXbxew5IIp7RddfeUV9J3quRF1wI0rGtJ58nIlc2Y45bOuaBkFbk2xqEwBZ3qi%2B6gkghh9PeQHJAwpuXMxwY6pgGqnDW2diYvLKFlFj%2BzGloNFHHF3e3vxx9dqfKwO61cSrJer3ag4l5A5p2ahA3V1OlCnGrHNSZiUXqrYj%2FH7x01KaleYZFJf0EBLaOtgJeWa0O7qpyUqIRViJlt3raF2anQ4i7znulK%2Fgtin2AFunpEZTIEEglorhwrVP%2Fb%2BQZYthJfkA%2FcZReQnVb7XBahHXlG4i394Le54IxHkVvWCWcTIJVbdS5H&X-Amz-Signature=d2dc8c9d210695964bfdd44561cd914d3476c5da5ccfe6daa87a87a5ec8476a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

