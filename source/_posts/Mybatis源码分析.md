---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UYHAUIW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3NNrKM%2BLXSmBXERpHP6z2MvenJCs2isbwCxJHk0jAJAIgUMfVgQUQSnmQ%2BXK6OlFl9aOpqyHyNKSAUx5W7rTUiOAq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDCYswQTq%2B%2FT1Ui%2FkACrcAzV6P2dxp3jVFTFVwuV%2FH3q0qbsPO0NvJuGmc9RHXs7nhh4936cExLbssaJPhM1%2BSMK6PVbBFgVCjpDdgw55tHpWbiDuAhv%2BhWhXH2M00D35qJpl1sqMFnbVqxlkjWN7eQHBJxKVx%2BOrXWnfMKDW%2F0bjsXw0FinboKeLSgi1pzMJqRzZx76zObNSOsYeW4aeOmq1xr6WAgfQ1iK8OvmMsSzB3X6rTjKa0jfW%2FSj4r7ocOnZUyLv%2B7sTFLLeMII5kCtd1mIOKvEVzQDzxcTJnVa5c5NBWrjhsrTKlJzPcK%2FD0CGRs9CTMNNwJ4PwFibmuAxOeIxTVAoaDYnB9Gp4Wh9TXTmHfSSP78jgDKqa2km%2BjkpB%2BhjsQYeorjGzLBcoW0BlvPjZZQHKR7FHGLZPx0uacQmyBvgU%2B9VLOQdZneqLm7Rs9t8vkHfTSQbmXuXgkowoMjmuxjXSUmSM14QpHQY4a7mqU3hZbKyveHwLT6wuPyohPAdvpRHxt03LOiYzO12qLeXPSnbphcv2uKY3NmvKX18MJ09OdCy7sZP8mX1g%2F9GH%2FsMopIefZoTV%2BlDzwOH10Bzebm%2BOTdp%2FZz4WASJHRJZnYLRqm8Wsdiz2N2pEH%2BYWfYT8J6fvNdH1wMOjnqcgGOqUBi%2BtNCBB9SRi5C1cdMc3gvGscIdY%2FrkjJLSFvv0WPeJ9u4YZwEFGEKsnLnuOu7%2BQTeQYn22x%2FXYgES%2FigR3H58ZKwWOTut5v32dOAmFvZ0gYL%2F5P78N6WyhTHZIjkLl6gD3IrNgn1OUTUnr4JdbaVPmKkZ6kRtYZNNaHFvjJUr0QRLzx7Ehwl0%2BYBK8HglMZFlXvMIu7rnbYA%2BBdroZWdLUMpgIhe&X-Amz-Signature=f4caaf977019e9fdd25c9023ecd182ad1a6c2d6be127616750c6a548e4ab6d71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

