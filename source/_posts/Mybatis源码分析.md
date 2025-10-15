---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWG7ABJN%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEr%2Bx%2FAbCKzez4zrWs%2BlXQKuXmDFjKpjKg2keg6vgww%2BAiBenvqF21BU4TRDMifgv9lotxYta%2BWEIfjrgH7f%2FiVphir%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIMOzlqRAm%2B1T7XvooxKtwDGauTBNGs93JQqvpurUdmG1%2FjZ%2B%2BhO7MeLzVdI1NVUEhoWHhD6dZ5HhuN7IrAZNpYhKYi45e%2B2iHaTgQNVA848rmvbCcD2uIr4U7jBiubPwPClN79EfonBKNiwWWGGiL53VeoxVSChw1OrK%2BRSdZ0qB9oGcWTSxRvNozMS5REibQtkQxtW2UVGhgV%2F8M54gl0nl13mTmpToNcJIXv3wXPq9AdvaUnr3avl5z2INhC5PLRVLQ5ubyN2R9PzwZPGRUTtLpb%2FnOxny3sakIKqBP%2FPkjbP0ZS6hlkjJaswWTK5PmjrZySJIRCFFPS0CAv1ZOuTPjbyHloWNVYk42aRTAQvmUH04re5T%2FzBM9FpWHlVPAVbz9KivCnIz949OsFf9hiZN%2FKtadhuEpPpRtUpRzwA7FbrAOuGiGzJ49d%2Bhkbqorf8INiy5a4VKqTrOvs9oPT%2B%2Bg7zRxfsnOSxpIN0wHPKomWeqGy2nW%2FynCxJsZs9M4OE5XrGeprPwzOfDbLuKOgLG8Up%2F59wjmHZVngZbu8ZxE%2BawmF9ySzVW4yeKyl5oA%2Bl4XhlEGtgeMwU2tOkBw52pL5eNfuGI%2F7WGtmJIIVHU1KzgPpRp3llZXr42Xrj4mcjnkoWpx2qjLLOKsw2ua7xwY6pgEY8WRbCUTpony3utw5f88CTsCXCGyhPyMEVlNM0iCxL%2FKk2HXg%2BSW8vg%2BKjyNOlhf68Rvoo2HWRKYaaUW6V0DF3pKlIlmJbc03fV1MCmTUoHkmxahjnmTnFD8AQEGqhgCVuyIIFr%2FeuxFju4Ylsp1kdJrozRGspGELnT03pnUEHWBbFBdlT1PN713hK%2FPU0OzKND8t7%2FiGACx2X%2FdT4w%2BKhzUmxlQl&X-Amz-Signature=1e7cced1cdd90199867a29388ec8e575d5e9c4baa35b70cc2d0b8471d2547ca5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

