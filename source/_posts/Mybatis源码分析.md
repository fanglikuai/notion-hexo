---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYXFGVZR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD6x2vX0sMFFpt5o66BVB%2BYC%2BoNrJ%2Fj7bRza6LpK1qD2wIhAPcSNoH4ETGhWygXA2lNLcn8p2mjk1v8wsImYidAvWUiKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyjIiNwdrStUvDXdtsq3AOv2Mf%2Fygd%2FASWAwQPdyqnr23UigoaFJc2s3IcyU2eLPF7fhgjXvAL8Nwbz7ncCI7vusGBfpEWzLX2sGh3u7FO0XJeyTqbxcQsknnsTth74D5v%2FdQxlKtwi4lQN7vMvcswZkUoSaV07SSBZNlazvXj5eMUWJ2e4L%2FZ3OPnfUeES6twZqVvzCql5xPrpOxxAzKaZuYkbK72H3r3X6gHftLH6q%2BaNmP00s8%2FEDV7Q2mxjdLkI2WbUJXQjVgHYQpWrjtxw5XqMb%2BNGWHfQg7LVFslHyqhNbeCYYUYdyGWneQ1jQXnbWYgbaIGfT9%2FENU6P93X4nDQhRUGrJ3v7npr4FnfcZMwwo9B2n%2BD9F%2F8wBSySa7x2Y2PyX%2B%2BXwBamY5wGyDooHibnGY8kFYl61PgsqNLPbqGOXH5Spm%2BrwmlQVr4JWf7lgGBukWSZBnt%2Fd9XRjMzICwVwSnT7JouoPbXhba9Wz0dgtTZoB7wweXhzPGETxWmkSC7uyAaX0aiu5lyrb%2FUVSbJxJaymrEbk5%2BKrFp6R%2F%2FUUUN6onVLSPNiyXJNtgxXdqU8%2FSMO9b7iwSNXRI1QlAb6TlLytzIUKrFrzU%2FaIIOKzcE%2F%2Bu8EgO35owSywLSDJBwioz%2B4r2rqwFjChuYTIBjqkAR06bd%2B%2F3N9PVbe0vNk9QzrZTkIB8e8oLpKfvN7xsrb6wAFRHjnv5UdTcyF%2BkHd3NysE5NS1jlhwqCUvC9AGvS4xLnmDAY2J4qJLOYNJjR2%2FICgS70lXmrjPBrTeJ%2BsNnK6ggsg%2BISEfVF8M1wVTowBlpbjnW2PguK9CF0W%2FP5mfWYlXmyAMCTF%2BYpQX8XIgLSeYKq45wxCKvTIEXRBE6HWMIY4u&X-Amz-Signature=4b809cdab2fea71fbbed8b9ce61cb873fc3822e92c271191b8ac569ea0682afb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

