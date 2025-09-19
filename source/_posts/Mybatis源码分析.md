---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBF5HX4E%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQCs3H0m7QevWf%2FbckKCImS90Xo3AoBjWTuyftAlODYm0AIhAJ7Kd2x39ZH6b4IPIlxsl8Wpp71L4jQckrl5tnf3VQJuKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzaUgilnfruUMjgnu4q3APK9FIAnO2C6th79vy4%2FHNwLu9UORNJ%2Bje1gyVdnCXTu3lig8xRqNLTrYi4PM0qBOE7kx%2FDcQQ%2FgbsrRJLaAKUfQaRH45Eam0WXvo6bMuF5pK3qBU%2BNeK6IHtqZx%2B8j%2B%2F09Xy8MApj4seVNRN5ovZMPrS9pLHJqeUMRpCf22u0s9wrEVq2f1nvdkuVCv5EpenF8u1LrE285Yv%2BcBSaO39%2B%2Ffmr2tJBD0P4nD7KZppe1pFE2neZJCIgvtxkqLCMrLBpWbsft5TpgqHJ9zv3xOfxAzTv7SiVf9femFcRYEkI0CBeBBybOc1P4fKYq%2FQF5wO3rUOD4wDeLSErnJV1ZwWYJ5P6ikgUnJxnM55RqAHBP5baOlNeHJP7sLiI21ievQb2SDQiOulrIpuAhy0gfRQc1Hed%2BJ7pa9MkgZX5sPYUk324Qel6vvrakAbuOfUvhGr9l69HZyj3SXj5T6%2BkCoMrvOf%2BrXtt5wBROnhCq%2FsS4ancO9DUw2bhqTZ8NE8oZoBmFbHah74xdzCnzlPbWajBbuVzlW%2BzRjfh5LdNTPmsxYOYdnIm4f5rNhhFwaICNEyNBmh7jXzW%2BXIJAnBZVaEmE3PZLyxz8qCkDLqT1fgmGI4I%2Fng9uV0xRs%2B0fvjClwLbGBjqkARa3wEbh4yg1xjGY2tzbnKAMdwooPuKeSh5nKofBO1Pn7vGpjfTTYBgINyclFOIUOLWUiaT0vqahJZ%2BjklAk1tb7FfecVsR2sjmZxXsBBZAYGCzdbZ3Pbvy3wsm%2B6t0Ha6WOBd1EejwjdDQb%2F6Z0KcbZTA8yP3rkUYLBV%2FoLE7TGzY2K8cncVwLa099b7UZ0sQLHbTON4f50bso5M6i462mbZ3u%2B&X-Amz-Signature=a71e3641cdb407838bf3d4b5f1a158245297d81d668d239101cae81f9b5ef016&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

