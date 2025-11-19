---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFIU7O6J%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBpyt4u46YyAiMkHBqqhOawA%2FevFO9m0KRk1mqrEe46%2FAiEAhlJKjPAt7p3txKfKAVWdo99hgzQKKYFqGaVbrf1ocrAqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBTYw5qtdZsw8G3EYSrcA2KRBZsnxoiWmN1I9g5K67mZOaMSCl26Ks4WbKGLlqNbpz8rJyYf7F9BemHoJFmeaP0oZr4%2F%2BxGdF8084B5m80Q11QFe7Di0uGc0OwOgPBsW4a0dqFQCI22nfn7Vz3PjusZj4ObaN2ShG8YGmrgAoIhTrTTSNIXC7I6l7xeHTVuJCQfvPWl12ornyB%2FtGCWg9pOgQWVDvwoGC315Geh54FeH1hsGpkqSlfi0s1Y%2F1okcKCbPBBqimesIn93mJhUbUlpBNo0lMvfSJb7M9B3HhKZy0rBPaHk8Lf%2BqdWvf5ozSCTpwknvfk72oUq%2FNjxLarR3dW4mq976xkEJSZyAu5891UmUnsca8mw%2F1%2FHJRNedTkWYf6X8HqY24tSYWqfr9m3ET1ritrcmnpvZySjWprCdSw8cz%2BqnnpBbqZzoDagwzHvfRmv2Z1Mmy3Us393p0YLE2buGJmzWGEpNeNsHIU3d7zn4mxmrF3jk%2BuGxlTJNw0csh3rtTIGuyd1S4bAqZi8VE5KIIYnzY1KRwCGcqNKE%2BuEv1GrCi9%2BUNBJheMPw6XtqaA2OpDxEHW60jZfCXX6NDHYmbk1MP6Wdnbi1BjCAP%2FFa3FHAzrsQ5OLwNoLT53AY2NreN4kG0hx0tMNeT98gGOqUB1EWWQzKNaut0kUo9byRe9AuIa1ecx3UWmsqBCXW6cWaEN%2BQ5rho8NS1YmlPHYywgfX4vLAeM5a3MFadqz6pm2%2F6ITMvLg2wmkgjFQQc7HX8VId2HbZcN9mvoEv9xSOZl%2BgDXoZALfi2oe7P78U84lOKpsdqYZboQdnUgHbCQrOdEM%2B471Unsj7pZ7xW6ToBWd%2FtoHOpGQnwSXR14Xg15B4CWdjqm&X-Amz-Signature=7b6f3f10f9d86b6589dcf2acbe71cefa886096e8f1195db3ca6d4b6542b06253&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

