---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SP256FNB%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHSHD%2BLy5yHyJNMiXLiZk6rfdB2j%2FakJQtKa00qaFUzbAiEAwz5LJHRtcfou3sUJQOZPm2v26PN7Di%2BXPis4KzpaMFYqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNZZRZRYRIzhAbtUeyrcA5iTbrR9OvfAcsvjQDy9RvD%2FIIi7rIHS6xe3kc0VhIy4fA%2FG9AUyVKQ3q4TW%2BToqMWTdsfMhh72GWbgrfdmqTqwhBwjl%2B278iMGAZ4w6AbHvjiRo002T4S%2FIWV751EhnbJ9gGscGJ2lMdJYuFGU7agkLy5XgJTZ8trNNxKerLwy5YcAUAJ5%2BgZuBbjCi8ATmTfHN4DMsHDXKOAyMJk1VTvCJXwAyLEh5CC7TbQjUYufh8FDVwZ6RwSUcoVoMuw2J409X0Drm3r6fek5XTfZJ7wzNpQXDMGwyYaoUI8UDD%2FTQ98wwaUGG3gRqgJDNnUHqHRLpNGGSKihtHWb13FcCk1wQzcwCqzOJh3T84zLXEeFCL2hky6SBIv0gh%2BmLyhy9G31IMduk3H%2BPQ2obY%2BHk1p9C1MxG0rSVYl0q90OAM3zLBlp5kRKliwHTfT84d7x0LkxiJtOq5u%2FrII%2BcjDUz8rCGyHONgNEWV%2Fb19V%2BYT8wSZHi%2FGfMDqi%2FwY9HyJJHtx48K0g9Sfh3bTfqFtI6R2%2Bs0nDB5Wwhwy3d3tEIBl4Ry34Eub8KYJ1td3IdmS1X6s3D%2FYYxC6PfFUSNhVMUy3NmSgzaA0zFdtBVRkOymeJh3i1WtXb21MMAc2BiSMPTD3sYGOqUBiCiVVk2bXRYaqGzE%2F1Ya8a5dQyVC70XXVOK5ex33Y4W3v4IWLcrAwDljN9MQkZOTFWQEsinEuB%2FbrWryPLbuhOxZLsi4URWFVF2Ur%2BimAcifnIiVbZSqvLFwtJO4voUOXGCp3NsmyhLeZIQyuDArHmq2j95MMS081iM%2FOfk1yY8ygw3Eq59lwecuF6PcHf%2B2mV5tCp2pfwrNqheV3h4cQZWqoG%2FS&X-Amz-Signature=8c111c43ac334874dfaabcd5993eb3fa267aa33cd119ba1b8df2e692b028420f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

