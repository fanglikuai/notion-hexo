---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKGM2XBM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIC5KArnmxZV9k%2BvtS3Fl3I7x6Z6YaRHSGERiqFzE6kK8AiEA9oYPuR7KnRiyjUYd2HrjlkGc%2FVUpqtCGYBTNJomChqUqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEeGLNgTbe6Zqi4CoSrcA6i%2FqaNmPWAuYtfBeKs6%2FT7bXjhEUdFNyLWNNlXa%2Br2crOIcIxuqjZUAOaHn1b15N5l3rmhzF2ZGLNlqwXeaJhZ5Czq5uE2ZI1EOlBRdHqXG08DZrbI8HPuiGoG2x%2B6lvOTc5lGOGqT%2BdXzqyCedOAgjll9PiOQD2TRfCQkNCCJMUhRa7UTKjLBdVm61HSEEJtdBaG68qlGSoZR2pwH84rQ%2FYEvHHsUGCD5k5CvNJBwAwk2VGmhNKUNvIC7rSYbs2vF0cNQ4LDtWwTKHJB%2FhrQTF8ZxAazWCScFCTOU3UrPsDeiIwv%2FmrObYT23DV9YW7%2BUNh%2BxiNiaPuMb9NeqZdzI9SEpA2tQO2aHw1nj3IqiTv4GBiQateKJfXzUDPhqWnImohXRWqbocc5BshpgYvckXeno1VGBGqgW9LKaibdcmSPHa%2BwF31DjJGuh2BT5MVZTXg2d7ygPDrcIW1NJZhqS%2FcIF1XD%2Fyke%2FXh21EgHLnmjVGXt7Q758Q3Nuk3BHpb37e29y3p1JFZYM2lUocqG7XWZtC5PtaEi90Y5yFWBHn3ULhCXu5bABwUE315NH3zCASsYJOjeOObToqiNYoOaO0ch93qen2V5mmP1KyBvQhsNcw8etjKZRwwmNxMPnxj8gGOqUBF22bz3KC%2FhJI6vJxq6EI7bvU1jkm6Yv1q6%2FAj1JXbq4sHMsFUQQ0GaPMoJdrZdtNQLHzdn34WgQU2p7hZ%2FxfUySGYSJZXZzXNIsp5gfexzLHaiU7kzU%2F%2Fr8veOazJu2ZWGEQbI4EzxghEYojipSlYfh7Tk%2B6qIupl9kzYAJK8asrQCxiuWx4QaVHLclxVI1fte7owi0RngZkilf3UqAtKfgjiCPw&X-Amz-Signature=1dcbbd143abd896b5446c8fa6c94ae8fc37a8c9fcd2d07736034ad129928491c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

