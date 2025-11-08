---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQLHMA4O%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T120119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIE79TOYD9tXCEykQVLD2qhc4QujYoq%2Bt%2B2wxEGZhubiEAiAlo5FCtm5slE0yzFeqX39JWDIPJY4CB2LhASmQeRV3qiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLIyAlJvnLn9fuZvVKtwDlCtB%2BqzbnjlaSnnsG49a47Sd4cpUqnlFj6gxUjRCyXJjNzfxKGDTlrxjkO7MWPtTxzYlUm9HQewu7WHPgsiovwZGMyk1rEymSOH2hKKTuxvtzESSJsvgAZWbu1AVB3nZvxwuC6YAWH5NNNh7yrkK2UjMBfp69NqS7agx9KjQgQyALWgRXL6pRJk8XNYwwz%2F7JCQavm%2BP3nU9yo8jumKsCA2kM10bm0bnQAM2LTHGo8kF9nk%2BgGWnAGmgaquFRgae9dHuQFYCjxtlQuQXRgdYAUttGwfX6Lnc18JxXuxcI39GaegdcCDIeLRg6eFJWA%2FB0etenlmlxm4aWPjOi%2FuNz6iNJjFotVT%2BqyfejleIeLggJHQL0T4skhgotRMo4a7a3iSR04MBPvRYUhw7eJglaKL8GKyQKOKQ%2BQ15iYnkAtS00AjO2q9EQWdNxtvNQi%2BVwt%2B1FA%2BJu2yDH6JfcljoNjGnNU6A3%2BIgdz%2FXRustiU1Oc9H025e1TjV%2B6RvyuapoHmx990T%2FGRTStRqGqC8qE2hnOMfWTzxWDhjF05TEcEyB%2FsZ9LIfOeLBs1YSty8bfOh4YiO%2BdFHX%2FEOYS4RAEXWNIJ8zemnbjA%2F81d3UxtOYPl%2BTlzFXwo39l1e0w5428yAY6pgFjse6Mvq6Bxg9wuGmhvQ8jjO6DIjs0f7vp4O1yy9Z%2F06iCPduY80%2F7C8oEOyRiJtz3AWIx5PwwhoNSX2H3r03%2F7c5bhvrXO7r0cCBeTZmXiFk5HHa%2BPwXCyN9j%2BMAi37OQBCh9MVCki1ChQycEEyyp%2Bpyq08qV4rXNkKvwHhcjPn%2BzEUIMli0%2BdjEzVarwmFs2sehoPwo2Syqy6Y4ud2f81s4nwtGs&X-Amz-Signature=55d315f4da47b043f9d5293b75a7c99859c2cbf267dba6fae24e469ea346ea95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

