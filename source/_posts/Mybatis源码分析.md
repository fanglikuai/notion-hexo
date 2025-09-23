---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DYZZ3UE%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7Z174uWBP4VvT%2FoFQWRQX1ABlQtMJe5S54WIA6zdJhwIgBGFXVZ4qYPk%2FmPwhgNLhnYovPYWaKbb%2BqKcye1jCsr0q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDAQee9jv7hKNEglj%2BircAywGes%2BhKHcRXyLspR%2FIx2W3Vf8TlgB2vEYfY0MF0e0gj50GOVDZwiyCXxuHfxNgTiC9wfZSBlJcecdPk8Alu1AR2kWA9QuAiDnjGLkv4jPf3WFum2xWbmtkJkfcb01SbhsYKTbc8W6G3fetdwP0R2AAipBBML8m1XEnlol5ULYv40d7PAsrO%2BIHydEPQRMtkKU5RZAjVtAbdD1RC4o6GEfi9SvEuBJ9%2B2RsO5U%2BpDPFD0%2BlOBZo1A3c4E%2BgQBGKQtatBSjTD%2FhGqebkzKWM4Qjwi%2B5de6L5XDw3zt2LpBf6FA%2BQKRSg2JUd8iVx%2B%2BxtecM95DV1KIw2hZwHuzJ8%2Bth2YQumdcRd9fODiYll9um6rKXK8gKS3SNaSIjiGm2J8iApddplLGTybmE%2FHURdkLnb9EiZArxL8RhSYiJK1S9f9z9ALPxpbf8%2FSmWano9hMlE3s0ZtCp2BuxzLyDajY0ZCnY27ed709kYfnau3Et6zwkac9Mi6w5c1yMZR6aQo6grKpzENbu5LzptGK5tACA4ueh6SLuIaM4G8yxwIIw3Wx5vU%2F0HQi04aincpea3czuHylN1wDDdv92WsK2M6xpTyKqXxHkU3gpTlCzoAsrh2nSHAQl3HkUHCAqxXMMyxysYGOqUBMMKkvccFTEJUHiQ%2FjeETAU%2BlH%2FnNxPmZCc0mdY1qeqpaEjdsHmg4ZyV2UZ%2B5mFGDPxEQyfAnYkLQaN2TDy8Bz9whul7I5tkEuyI4anJd3LPhspis%2Bh9rpDsD4p%2F9%2FSYaJ0SNyeygAOarj1HKJUe9cWmfZe6%2FzcvyC1BvAswdYZTnHdkLnHgVvNpu8knbOVZ3vifNkKsoonLjBnhHnMGQzGXRe8tL&X-Amz-Signature=55cf73fd35a38ed590c9884a4f67c69b96d95b1fc89b369ca42a075ee896246e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

