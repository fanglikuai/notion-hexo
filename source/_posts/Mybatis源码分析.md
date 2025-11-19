---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JQ6V3Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCu%2FfQXvtkmZSpVYNVYYieACJiux1AGM%2BQkwMIS5ujOBwIhALPvP3I020sEUXxgy25L77PJDKsiXt9UqYI%2B3C9a0iKKKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxujie8dxHRIKYFuPYq3APIn8Qm%2FVyX4sWqSmAxu49QP%2F8bUWcWcegsaouFzGOMXLRKksof3YDB0gbegDNRJkmYlIi54Mj7hrIG09dFziQk%2B7IE%2B1VXNf01IXF1sWlfaLjnhxUpi4iqepsSwsBkNacJP0%2F81ECOicxaOp%2Fkn0EU%2B4Fed8lhkj%2FgoLMLsED3JoF6n5HEJ2mGEdjqPYRbIlZP2%2FlHAvMTAJocHXExh1gEn02TUCUt21%2FrliP7wOYWuFBqvGu4cWVa0Xdx6TKM77PlhNlbpdyVKcHK7vjzzy0Mb1%2FOXynW1XTa9NJtk%2FoTKbjTVeIhJD6LxdBNE%2FXj4uDbAvQPVij%2FCIb4W5RXfo2rQqblKUUrrGiNKYo6q%2FDQRTFNw7VYtUU7Ys79qBKdr9t9EFkFIUU0I%2B7siwOtc8bwt1H7vuSLp8TYec%2F0vNxxG5IhrW%2BnVnBZr37cgVcLNRK7e9gISJfiuaGEONZ6pfY12UclzJVIPTOX%2FFVfQRdLhuDL4ENrpimFSW2oFsD8vlHR3N480zA5dMtfvquRy6tXqmCvIBzWZ974io7YcI6Raq2Ojzq5X1zp86wj4ONNDtV2A6se7UU0PCWmbnlHKRQtRgNWw6%2Bso%2F4vfN6U8uAnJpv%2B1u4hkmRnQ3FURDCJuvjIBjqkAY%2B9fY%2Fca4k0LPlCNOCzAMY3qkQgT91xOpzG5xkcnysitMCpcj3%2BbmwTDplc4yi06AqUoqtD4Rea2j36ljPGffOt6RnZr46Mn%2BUZzkSscIq3igQ%2BZeSisvovgxHY8m0tLqgC%2BaiuCxUhh6kPThJtIlwkH210QuGPfyKg8V%2FWILZo4bYChr8US4YknCxTfX1%2Be5kOYRtdJ2vJE4n1jR8Qv5MzlrGj&X-Amz-Signature=5dd0b552863b0e073ab31d6acc019de2eb9082c97a14af5caa9fe0971b2556ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

