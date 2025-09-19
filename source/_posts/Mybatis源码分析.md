---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2V7VOLQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCQB4GQ72634WcbM7VURxyLuDhaujhS%2FpXTEgYb6BuG0AIgYgmVvizYgM3surNmv6018dQ6x6IGWI48AD9FRRig6eMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBGwaWUwaMZANYkEdyrcAwJUnEMe485HiuIg8opDRlE2ZoNrfYz6pzU%2BsW9mp01R1qOE3xLY4WCdqugb0zig0wKQk1V5qt9Gv23qh6UwdIYcOUrDwHeKrLDIQnpRYNbabVPF4favNn92dXTy0pV5gb782ISxq6cNsC0d3Tus52qx8eXSlzivwgEWN%2BA2H2hi%2B%2Bl6AA%2B1R0Fgf6cORs3efHv34hQupe%2BR5%2B7UpPBrbwyDLD%2Bvtk12dpD48k9TAeQo8ENhsx4RWeamzfRnknV%2BoVHi65f8oivhXpgEn%2B3xt6SLd%2BJsWFRHae2goWuPZMiHvwRNvzoNuFz4IMpOC29eyTdznJO7oExiBmGxnnGzdGpJPeAtuPkFfhx7%2BqipZxKO%2BBS3m5%2FBHfNzIFa3fee%2FIq6o6Y%2BzZhYGTAOjDZzAJWJX5Wjgg9RzSsYIwLjWGd1pOMGtCOGvkLNi667px52CNQcePATWHAwd5XKvZ9RuI0qjhyx5aTGvj4Y%2F8CMX3OA9Gc2Soqfn5Pkv3ypBKt%2BJA1L4Wg0bMMaBODJflfuh%2FS9ayR9LmgZFtxd6m2LdYX5kNxwWUVNIs5D5oZ0ggRMBUvrPqAUG%2FygSszsClXvhU32Gz8kz4%2BBJh1y3bwLYdIHbWBXNIz2sD1OHqF7AMNaKtsYGOqUBoy2LVxhYGq5dWtF2klvyIFSuQ7F8b4Jgpu%2FYLfvWawuvKe0u%2Frxmit6tgq4XoGMoAPVYkG56BUQ0tAVRI%2Bu2k8vwBMMlYCnfvufniJHMvsi9rvOmjwZ7OSkHdYaP488LPBqnWaOH0OqsY5ckJMlxCDmWrLEqNSpBnyOS0cQLKqEi8AxkxRa552CdFzZly3nDV1qhhiDfw9jvx7K6xMGsMJqXUvmX&X-Amz-Signature=9281051df7e3577213d3058d70455b9c85de7fd9fab286bcbe9ecda663680b11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

