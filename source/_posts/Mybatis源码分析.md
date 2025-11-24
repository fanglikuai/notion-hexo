---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDCOJYKV%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiNWV1Z89%2Bt0j2t%2FRkVq9cLZP3LHJG44pu%2F2R0YtRpJwIhAKNV6L7BezEtvu7jcuAFJMQn3q22XeZvGiISS0BV0r6jKv8DCE4QABoMNjM3NDIzMTgzODA1Igxh4CeqCdDfEWMMh64q3AOYbGNF2Cb3G%2F2qc6KdHunPJ0tUnzYLLMbn5nlokYoK%2FvjpFoVEeo2x%2F5XwOs2iD9JEnqZ9kltge7%2BGr0dxVea1RfUfMA1ORsCdWTaBcyfYy%2FfBPqTqlVTYu%2BJfkjCbN%2FuBuluCKVkh1aRkOc8xWvpRizNRSbgmYnhOZLf1zuz18KXT2rY9Yss6fKxBhmz%2FWiJlT3IKySEVS6Qemi3ZksKHJqtILAxYvgMemZnQ1VNjBLNoaq9QDyyACri%2BaIX67A8YEWhrdMY0L2ZI8RZ8KJQrngx0oK2ip2W6rCiTPRNzlPmZ6Tfx4EkDj29QavngI1TzqK0dFg672Tgn1d2TMWdu7EMTDFXxwJulMzTwXCyi5W9TbuJQfKV15rIhxD7Tj9OrmHnRED0yKhvBlSB7Ot5Fe2xPzjq8EFWn1hHlN6Kdi35iypHzwyeEQMO1Aja%2BtvlTXl6FF4z9zOLQhO47LX0Bg6UguGnlQaUd%2BeeoSpPjrxPiiAapkqYzAgE%2FGjAZH23KlihL3VPbNrriJR0qb3wZHfOLxfWulxBg5BuPcAEt2%2FjhfmrMQ%2FXRjDYde%2FPG%2FhVI4y8p%2BrXBxzzA3Bkbj3SvRMPUE9dXQLV5SnBLNU49qoZvGmICvdrPKAD2vDDyyY%2FJBjqkAanJEFQVmqbuQv3GVXFvhDuP5llxutXX0SiRnDcaCx0XEGRKDrROuMocD9hMNMB6BYp8G%2BD4fOk%2BU95kw1eEjfaOjuREXm0AHrQ0Rnt1kJwsUVBqYrdka25QbTzGd2dwe8%2FOJ7exQgl%2BmbttGcMUy64WUg6dtFmd3tKAqm4sStt2DPbnoTPHH%2BteSYfjGeCt7FZeapmaZ2aObQc41V3kqK4rvm1d&X-Amz-Signature=56f4d2498bdc4b2a6c4af70a20d0aa4b85cdf9ee2b3621d6ffe0a485c29b6c28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

