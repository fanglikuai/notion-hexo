---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYCZQ3KY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHLEfsEqsgT8Tn6AiGICjOWoRraZ6JnlUfqIuO3ohVunAiA6o0qNgUlohB51H7p%2B4NMa7QZwdOjJgT%2B10TM0RoO24iqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMgb7R8LFo2%2FpVJceiKtwDEsHPH5swfTDasqy88FRPmTeYFBvudKs1sEdzTBA4t%2BYbCbjxEIce22BDkMRBKx8oTbyZ%2FXl9sDYelBLx5CWdG4Rj8CX2QtosrsOTg7KL%2BhSvGyCbBeHIJ6DC1BeficgX0dBQA40olskK6PTV%2BHq2UtVLpWczMuk5VVb54fMeAfTvz2GCWG8b6J72N%2FH2sTMpWqokQRg%2FbBJoK9naNDp1hEd%2FKFYcDfbbXcCR2%2FS5gkWB%2B9pa3293s7HKnV%2FJhXpp1DPiMeQRD51jZuJjLKGboMlXwobRtAbJd0mF%2FA5%2BjKPLTiOwfgCig%2BMB6RDN4sT87%2FIS%2B0R8F%2FzCp6QD7LlnnHhkF9AqqGUGyqMViGwrCciKaK8%2FW0KtEAv7br2yqiGFxushc0c%2BY44ANzmc0kQDfpURpP5TUi7UGGU0UwOyYkQXk%2FVeioX6LrsJPYA996FPy5Wo%2BZibEVojPY3Tclo8zTkdWm%2BQKYTjgSgpKq7uESHu7fDKJBgi7FWmM54%2BNLePJf5larIQajPqqa9WjZg%2FReqAoKFKYJtuSwhnIeWHZvIv5xl5y4inRfwrzmR7Gdyx80prV6MKs%2FEJ2ErWFHGtS5OD5S0GTjm1viL02Bk4JllA77kfNipiDLeP458w4tbUxwY6pgEST9sQsNrc3VMnyUPrnzqBEs8YtcLCB7F3DJg5D7YEqSDFW2filhykcNXciJE%2BBRI4z8JtfQb2COOWmgC3v8f5hbPxV11nI0dr%2FSPkOiJhC29EVzAZ00TV%2FFfjtpUgrcJ7F9FT8idjTA96GHvClbLhgeSKG1h3CiT4yVT1yZOO6h0WfrgbOC8nsxpMZZemxbT9%2Fb4WJoOGJFXKlqdSdY3es40asLMU&X-Amz-Signature=813a67102daad12509190c422a88bd70f18f1f3f818ba3756df990865091b406&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

