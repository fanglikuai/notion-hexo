---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSNHKA56%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T130044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCICsBng5aKj%2Besx1IigHMawp0qar2DI9wATdcOIfnephwAiEA7f24VMKiPEimYH0L9KRF3OapmSjugSF9YI%2FaDqtzbkoq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDLjBoXYoA66HJ%2Fi8CSrcA9Xw0fVwmDL2fKn1kXYHWlyE7JErWZBvozv7AlUV0C0mrhUywjWSQIQTjhTmXlRV0J%2FuxJPF21JD4ZgCb9I8x8OrWDLJTW2mLkUc%2BzXlHOD9Dl8u98FFS6wyRbe057SiGviDQ6mrwabk%2F2eunUEqF%2FsXS%2BolbLH2a4TGVWaIvaYMKaI8LBNuqtPKoPP50D%2F%2F0r5sPwPA%2BvtQx5DfWwQsM4vYjpPINvY0Jw5DQDB9ZUqiatSnyM7SbmPR3gTMjNsHAvco6q9a7oYXwaM2R0TX83Jl7byzRhdqa5K80Ruh2r5Gua6mCnKc7OhQ%2B6tBpqReclK3a2cjPAGD6YQmQUnsTgNcX7yP1Nkx9Z2sBVKa1FHwsR26aL7bq2m20v5RbU4dssI80jskvOijQJ%2FynLsJ2ZYB5w17Bq4%2BGkNyr6AEoBoC6%2B%2B4CyWPQJ4p%2BSXzjJThDroRCa3mBPVet3vUqIDKAsGVLcVG8OA2UlLCMLJjqoidExBKCDStFMTcsDd%2BfX92N6Ry8bGWK19h%2BOqDpRK35zVGRiXoY33weQr%2BbZF1fdJVbPuXgrAoP1EzXj%2BDsS7yrRnuVnS0iHS84%2FI7Xf0QpSTgfcPaxMbiCbotydQwGzRrsKgxAA8gKSVt8Z%2BeMIDbksgGOqUBUaitUYUhbtzOs%2Bdeh5QTa7yInMZLX8it3Mnj5zXRHYf%2BBPajGVe00YT02bPXPGFQ%2F%2B0TYu2QyFiNvmuLbtrGeqFWRfbINRgPmp3fUIHZVqZQIWfmM1E%2BMjEycTGrls7XC9ztkWCkCZ1VJmR6sHGbDByrrhRVw4rEVlu00noQDi5IAcnahMEfHmPk8nyJBeg%2B92DzTQyyUOpurWXgE%2FKP0rBSg524&X-Amz-Signature=c69c2e68ba72cb75c37b8731d26b4e34834b4f49b65b548c01e5d7684aa16f07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

