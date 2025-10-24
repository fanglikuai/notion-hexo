---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTW3GOK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFGbMXy%2B5N52mxIjIEj%2BvENK79zhVGpm2e3ulzOONvTgAiBTCk1XmcfagjB96cJbL6py20SAFueBMXThN3MVEHl6Cir%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMWYAZMMBmpP3O%2FwpyKtwDGrb367D6h4V7rpYyaN4795Or7exJ%2BwdH8v54yfCMZ8%2F7gkpahj22f4lhzlkRPoRIAlRTqAgWMZTw9eLLD5dUJoXLF%2FWJM4UMgNhLSeVtpXaTYFVU2D2Anwdq4ImID8PKPFK1I4JznxpwfffRUp8LX6JEINTp%2B6kFiI75RgN1B62HGEGMjCgKYbt6vtGeUY5dYN2lPS%2FNEjz2eFJKgOc1p47%2FTtP8FaGA4N%2B%2BftEup56der0Ih%2FFEGSlhU89Q5IHwX7D%2FCa8cog4DliJXB77ulECliQwPSGVHfu4Vq28uirPIYOkw3bKAfnmjaLlOOCdatEgyu67bUhAgA6uBt9NDVnKAAPTxfsaxk%2BHMMJhqQaChXq0WNk9%2B582%2BMBHPfDs2iuWKQ9zFGRkKL1NZdAEPk%2BGG6Keu1IW9RCPO%2FcKShlRboMC3GTNvSWOh4RxUzHUfWv5uC9fPVdDOC80CEBlNP0rsaX1rYez0idCOJxKu2Re0bBHgWoAhHNMuyCxZET5qFn8DOuPtBqs34X0Nx15eChn%2B3ibWjwz5TuBTfmJa0myHyxOskz0yG%2BjbHe0ycuPXnmpGBtTO8ckb5JDxJMtRgvEbncFk4yYH30jht7zXyTpVroVstZxWbO9B4P4wr%2FrvxwY6pgFcjzedLpzloWV4C%2F2z24YeBRoHq1GoCgRzbLybh9Eu%2F%2Fy4brU%2BgUpdaIo%2BD8s5l%2Fd2bPq7FwBFx8K44NTLR9Nu%2FBCH90HSkMOeSd8fxxcp%2BIf3J5v9p68ZpHhh1qBjxfd6gSArVZ2FwoBtGCVnbTFS6eWthNELkS1uD%2BURfr2VzBs8ZVzK2A5JazGcHkkEdY4QSYPia0lZBD94rbwRMwQVrs6XP3YQ&X-Amz-Signature=81b0f69ec1314f1c0b16bc56769c125f3faf683106b962842cf555b3a4c7ac8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

