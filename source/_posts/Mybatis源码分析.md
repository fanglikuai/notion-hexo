---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HLV5R6H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFWXXe54np6Tp%2FNPjDba1lour1Gj%2FN8ruV2tNbX4MchdAiAz9QFGQnXOJbDMxqC1tUXipMcSKzeGNom9DrFGY0IeLSr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMch6mDaOdgRr%2BCAidKtwD4U3gAIoJ6neRoLZUNaPgadYf60jUBJmxrrmlLj3qaGgSkxctkitJ5S52FLvcKe6MbuAlsOqE7tMaYDIT3J0w%2BxUtkx8p35SKoM5SZBn3nQv9etWhoNfF3dxBzpfTQBB8jY7DGzyyBp%2FQcchamG4zfTAQS5Hla7TcQDag3RlvS9fUApmGGlaQRtSc1XIQoue4xjgdiiKC2WdXfsDVF2XkXAUgKTec0jkiFj9aaAp7qwKXMUeNUNyBY7GRsB%2BPSeRyZ0nUnvUE5lmT680%2BZNefAABz72%2F3dqTo%2F03ftAl1pNzb%2BojykVYzsizMa3PqK7gFIddVjTl99zvrAIehgXheYm4GlsrdaVrOBh52gdzdGDjUWBpr3qrXTZOOQfRBuUSZ9kgMLBw72crKTVe370Nr0HdMH4oCOPCaSgsyrTIY8D7IhpvoEmjVGpTGwFLD8L8E9t1WjiUV2kIKLh6YZKxJL2SGoNiBkkd7DhYzI5iLS243jykGqzf8a%2BkZ8sgzybcOY681NDr3f0b5WJEXxQ0%2Blo%2BTxc9hOEIVKNT3zi58tn3GiufHvYRKlwObODI92Yji%2FDxa5%2BkxlJgOJoWEKsckNuYoSwf4c5Xp02r6S4THNIoJA6d5v5J00Q3%2BYtEw66CJxwY6pgE6Igub4NRRMfrRVVHkejevFrFQDSD5b4SxRgTiBTwF09EY8W%2FwXGLjZQ2LLZ7wCQ%2FgDauNFMiWCymPSNGQi6KXCYIPjvVc6bE1%2FWV%2BY3ats1XUGVE67T%2FX8vYP1TnHw8Q2sB3NaMaPz%2Fao9u%2FH1yONU7IyZfUdGjc0EbdOCsx6Rt3PBg68khb7%2F5rdFkt5739hrrSsRgnL%2BD7jJOXtWy8Cd9BuupgP&X-Amz-Signature=75146f30e4709919c865f621bfc4322ae9be5b47e27a0dfc238d284e404c10da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

