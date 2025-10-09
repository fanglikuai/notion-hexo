---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GUBURXN%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T130043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJIMEYCIQCFgoeUMi3G77u4oMt9q664KFC6lFQ7FtlmHU76Ty4GjQIhAOsbX5mVqUPoCdFj1rHft0AppkEs1lj0Gz3jf2Xi28NBKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BAGaSHNDGFBq4xo0q3AN%2BbU9ht016lq012wKcRJ0YTRjhOdI1%2BIZn275BqJZE%2Bky6l%2BrYrs55KctXXazCxr7ncleeuucuUWws7g1oTy5Tx04sISTMJVT3R45C5awRxsXNfjLUfLg6Q%2FeyKOokU1EThoy6GpI%2BZYmY6xs%2BK8i0nCywprTjzDN2eA9pBAAeNMl4VWlGv9UIGbFhwN1TLJ%2FXqSUOLl44uMlp7Ld6tKOo3eJN8VKz%2FSGHEe5lN3CMFoN3fYDTTmwoSXhLR5lhvBUs85VqWk4l%2BkJ%2FuPPfNbqJU%2F6YBu9%2FOoJoYofhbVr1C5M1wxTi6CBpr8mj0NnkFjeeMLoAGzTKSFxxLVlpGwVyNgFaQWkQBGFOOdB4%2BNa0f4rfYcLeOfTjlQYcyNwCPEIf6%2FZBDvkJmT9mp9kw%2BK5KSVqNc8KStzDhOUxUFUJWvX%2FTlkK6LPg74rIvgHmWfnTo77bGagPhC5pBPNi9dhsrZGw73K9Kx3UIfkIlrxkTCGzzkhqeUjgY50C8sJCHeo7r0ofERr3XU6Y3oBaBvzq8qbIzGLfXsBNubC2L6krxTMcTdfsWbmhVSjIRn9sYFTwqz2R4ibn7Y2XFl7Bij28PvLA%2BNVhN4WpyIVMbubNvu8ipVgwyPE9hEPFxxDCdwZ7HBjqkAdnqGg3BRyrkrw8G0Lo0oWtLrX45QM75%2FCNw%2BEZODEDWlIVM5yWuoPnwAlzqTT2%2BbN74hWfz7W65dFd5fM2eWqw46ZVF9dT9xSrk%2FnX23PVng%2FPG%2BA3hS4kdWsFlHfTxdcrhJiQXoY5BHtS%2B9VNcbr2b3DbBrJayfv5GrmnFOlBgBaBNhN4csHRFdnMvvC93YStPAVGMtlt7z6oGOEJCTWNmoHFl&X-Amz-Signature=307d8ce5df203fd7862958a05ef13d337a0fc335acc5d5e56d4207575a670114&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

