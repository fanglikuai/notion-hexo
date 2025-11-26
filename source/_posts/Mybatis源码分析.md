---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HIIG4IZ%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjPqbcfmdLa3nYF6N43%2BEkxhQJPHfyTPOY7pwfxSpHDwIhAJWCvuL6QFXT3bWz%2FEV%2BulT4xgg7JGYc%2BQ3P8Nh76a7BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyu%2FaGd88b%2BgL8RoeMq3AOaHAHzR6jGwjZ1Sj1Xo2g0Sg3lIJb9pF1pRUCTTNdWyoC155HQ6ghhirrojqLMSuEblzl89MhYuKha7wQr65KJ%2BmEq1dOTa13HADadHBZ48j6kWn2ZSkBRXSQhF4P2HEFQyGLv3dNs5o4zO14uEIgzEGG6fEMWqb9fisfE47LN9QS1e85RBCTJwBMVEpBEfH4fh8mBqivYEw2%2BkDID9Zq%2FaclV%2Bx9AZ%2Fxln5UA%2BI9ZdqaQ7LLwx4oziHiqrgylK%2BkE5sPFV3kLEZ0vwYfIxDPJUSpXHzpLPM16lFjr2I8qdTDBmjAMD%2F%2FzgY3BwO3P2Sl6LS2jX%2FQyJyGC7KpmVM2rKt4oZe8YhdkH0cx%2BJ3FGmCXTJNTqnUnhbWT%2FzJWFg1O3eferPjFCFRz6LGzsNwAVnryR30LdIhswyHvH56ThGsguO3nO%2Bj81Wfae7iYGZ5xN1cduzz3Ox4D6nMU5eoxeX9QnEWmNxkE7OQzKhkec8siqd4b1Y1giNyIqRM%2FYwxUi%2FDNFVeqH%2FQZXyrdpnYJBvtZ0tOEk4O8J4InFjFYCIf28CVK3yuBONUpsDO27IqOFBzJZbuegUnffVyySG7G218cUhbqo5nMfre%2BAlawle2zg9gFbo5NGTsNDLTD17ZvJBjqkARwyfEHiN8SZLt4h%2BT%2Bzs9F0X%2Bmt5w2cP3fPkuA8TcRanDZrOw%2F1rdLiCIlmtwSAGxDEZuCYGe00c8%2F5mz5qQBt4efGEl2Nk7ldKLyj3bXGZf4MlEy0xWDhfrY858TBashk2sR1802GIpC1tmtTPxI1eSWwvOgxpbIhgsr5j0%2ByxG5pYoE7%2BG0%2BzdAskSFtwPrd%2BjNVCu8J2D%2Fbbx%2BICPUQeZyPB&X-Amz-Signature=b5f29b7c5944e6b7d7a9e8db67258bcb1ef30037f421ee4860ad699daa913431&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

