---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXTDXBXC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJwT2%2F0l4LN%2FCVRALZG%2FKQGULC1Ui7rZBL9BUgqWORVAIgDa%2FalpNVngXH416Cz3vT%2BVSzsytF%2F08QUEhLYFRCN8sq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDF1XdEBUAeI0KezWSyrcA6vOVGkM0OqTOLZj3hRHQnd8yh0z9fd1gXzyqmmCvMQRda0WAJRoXY3uQVYQQDWKPHlRuBOWghjqfBOTrNIU%2F3Q6jtXgCfQSjqonbkLwDAUz2Jnypz0aQ76S3QXnTYk3k1zKF9iuELPxIOQp%2BlSig8aUXyJNd058IElCBoJVjV8TOpGa2RxU9B0ppuTTKdDBMM2J11%2FuqUgNzFgEWicjfq%2Fe7ua5tRIrPng8KLw43r9ckcdWvcsFfFt6jQNUaQIQh6l5%2Bu1Tuxp1YT1PfCrzWV3yZIKIQ%2FgObubJ%2FhJ%2Bt1EqVRoShHKXzHZIagXnbJbSerbyDNQ9mn8gCnpQXtBnLrJHltyV%2B92A0guN9FBBxEyTT3Cw2JkKfg0sKVXi761X753NtxtgHBU%2F1E5DnHKKF3LFWSMNE3JUj61I79GPIemPxMVAU%2FVtizOvNkkToIXRmXuU9p8l3MKgEspwqAC%2B2BNVg3BIKy7gEl%2FsY8eZw9RAUmSS%2B4XULLZCCpKtOMzvlgleOxxstEJsPdNQMqHcPQp3RdTDPWE6D2GvqjkY41rhWMrn9C0nI%2F728SCnW1u8lK4MnrNwnIJZfHWUuDm7b7vkW5ZrztK9yBCcviyDomtm1seR5dAs7Qwa9PcSMMjnoMgGOqUBw%2BrFeeaS0GqF50VhMUZKIW4CDj8irwedOZzos%2FesEZvjytSTMS6BWW2%2BgkH7uil48Bz9HHN%2BfHG6tU9sllZyce%2FBknMXOMNSeS7KWZ34ubjYlBhndwWn%2FierHsOTDjERT%2FfVp1YSqt%2FV8L%2BeXPYdQZoJIjZrIW8rkMLubqshG5AVqom5JAFvSWmV4xCfGikTiWEJqfgCLQsYLLp8Si4PZ1yiH9FJ&X-Amz-Signature=60485a94e29c5da978e0b27ee69ab027328c51c100c882e432ca72331db91564&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

