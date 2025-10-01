---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRIRXYFJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCn9NKKpOwJMxLr%2B5eXUbtmtrmOv%2FYC8q3pYuuR8EOCqAIhAP1gYZ%2F7bMzfc5cTN1XFsPUbZDvCAa2fTlZbkTWC9YhvKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwhJXkxQkFDVwiqf7Eq3AMpDdjAnginkMF5No69VzbslXIc48Q%2FVY6jWIjpcvlEkaGhvE2LHZbzfmUmzIQxuR8hTEE%2BUG2bwnlIgKAUUq5Y4nkFQ4wa3rZ4NEAxC%2FofWmwb4UUHl4WiAI90Yse%2FDOiu02knFqnRagYoBxobXA%2B1UD06Bn73%2B18P56k%2BT9mh%2BfvxGLqi%2BtQG6T0hIwdnDVakD5cdniWLl%2BtRQzns0zHoGrm6HUyJtde4PtBfYjG0wDuVGmQH5XsaFutZrJtv8koquR7EuxPV81liyK%2FVhDt0S%2BuQefJrSHDpEbMV%2FXSP75vW7wixZjBlKCdr4BBe9xqZ3Qwh%2Bb%2BnohiV2Cxt3LA4rBoclgegKWG6raGHKJdve%2Bynw%2Fv8afvg4DpGg8WxxZY1l3hVstqhcXGHKwECMxy%2Fu6kSxMmandwAMj%2Fc400ut4oLJY%2FexzyyCY9RuQ5gUtajKRp2KThtlYo3eyEJ7kcam0jmv7ksYXG7sQ5IBkvp6kkWCGSpnbm5tLipJt4tqbHKDm0lYbntE%2Bo07ccnVdNzVUk5u7%2B1Q%2B3S%2B9vI%2BSP%2BCxPboYH3mvoUxS9jOTW%2BCmbMoX28O6nI9vYvfI5Y4oDFGwfRGLDa9852MciS5nPjHxIg%2BpJi%2Bd%2F21Z%2FdnTCp7vLGBjqkAegtjYxKeYJ0D6O2RZKh2%2FXOZCoVbKYHot%2B4%2Fb0KiRgEXnADp7dFa4pyetC5EfwMKnM%2FuizMcEJuAy2gvrGcP4w6A1A%2FMrCPTlSK3l0qjAT7EcCrKHtSpSj65WJ7UvPilBccQkKpAnOLkcvFB73lXTneG9AVvogle6xUJ54pMtq0Oip3XKzTwKh%2B1CQkBSX7ftGfQXd2HawqUYa1j%2BUy5BI%2BngFi&X-Amz-Signature=0766f3989fa7bf607dad3e9dd4c9b253c812c4b5d0d5d3de65f0ca4b82a69fb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

