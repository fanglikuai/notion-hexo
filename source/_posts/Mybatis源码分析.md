---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYKDYGUN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQC%2Fp%2FLXtEQ8Pn0vE4n2rAtouTePQQ8WSBYaATAlRkyIaQIhANm09tiqXWMsDSnpmITUcIo5w4uk9%2BQ9wUHcat9dEYXGKv8DCCQQABoMNjM3NDIzMTgzODA1IgytHA4dWze57D0ZlUwq3AOluFP2ZaMhfU%2BqnDF9QgzVSdgA2cH%2BILrL4boOjqBZJyuGiLFJKqjb5oscd%2BPHyVPfYs9tRgPI5AOgvjTvYFz2Tw3ZD5JPKoT9QOX%2Fqc8aEcrv8IWuILL52OesF9kamtjKO8o%2FrQdgbF1nABu3TcuBbBgoo0c9noi6Hzs0AkQvMGhx72JHdnXKOL4qQfmaowR2Itv%2BwblmjgpZgUY%2BOAuQyAAJTgGB7G8XEBxGMFsAU0UjJRPriK8gsa0dcByM3MKpBhIHdxR34lcxkzjZoxaTa1A%2Bz68RPfc0Qdv79t5IzrXZlxcZlk9Kmjof5dVe0Gm2ppLGVYnPqAMkvWXKxoqp0j0VLuRwSvvRHpFO3gI5%2FHQZth39%2BJYcMeOmWxnZ1bMZvCpm5Rw6jQiOlPWFUWUySMZKILsGWejyvogJ8kQ8uTl4A6RX9%2BzzGxYz0cq7w94ZJpC5%2BbbX8u2CzcrSC6wnfztweyvM%2FaYFs73wL9l%2FbX1AEk0kU1Y6lRSF7A10cwxHrepLnHSgyVyiVAkyApmwRdRVOQel4EeS3WU4jqtkqkTr%2BCuVeJNOt1YI9b%2BTcy1SC778ExHdIetmg%2B6oSXPDAi77q6pzFOR7sNfOhuwrcOceIssnTxy3MyjhAzCVoYbJBjqkAd6uLCA8EMYaw%2BKjKjTdMVB6%2Fu4xYr8w95LjH5oItjT0pKKrnGvvsddlFZyic799A4KSrzmffr2Tzph0OpO%2BqVHovKxJcs11t3PIKWAEqwSfG%2B2WKCnC6zqcMuR80DTTdvw6T78LwFtUmhW%2FyfibAlzLX0%2BQmLjk6CrTIJyX0BHacagtYmNEyb6v1X8PWQtiCxcaRoGKZ3YbWGdr9Ke%2FBAm0bGVY&X-Amz-Signature=af3670250c44e949952e2004d5af6440fc4af42145b66e4f66156813e2da6f6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

