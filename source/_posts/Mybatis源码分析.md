---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DEXLHFS%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQDDSAU%2FwzFjxt5S%2BV%2FcexH21%2BYb41jq2Aokh6MT7pqpEAIhAPCII%2Fzktvn9pDp7RQInHGUve7kGBaim7HUFMdRTwvnUKv8DCCgQABoMNjM3NDIzMTgzODA1Igx%2F%2FWavk%2B27W3YsRqoq3AO9mP%2B%2B9vm7HGshMvsR3AHQD3B4xQHbl8thFdUyiuX16yqM3CEcnZGWFcabnZhVzXa4cU4H%2FHwrXpQ4wdKhjIB8a0oIdPMzxR1BnXLNpdD0%2BtA7ZlovodbjA4MPZcua7APUSxYiqRgVgWubZWvhM8V34lQjUm5XZlp%2B5vSkaDU2CVUtHukMXCf0SrqL1v7ELksSyt3nMOVKtCMyMf3twvJlsdw3V5pkpbhJEjrHnqHOd9xtp9a8nrEkZAcOKYY8yuTQCpkf2sN1weJk1UWavPBSIG4KkNch7FQRiUCFfK%2BsCb3hVDnFGwm9JbEKL9QrMhdA5QfagW9wUdHd9MHwRjHgy7TjeKHp8S77AweE9G9mnnsJh7y1Z%2FZ8V1kDB5%2BomfuZ7tZiSWjwITIgUVAiKp%2Bks4nsHKKi26QTWA3OBbDFNCutuIr1Z%2B92QPUO408YAy3uq8ckaoptYGDAomEUfnsxbyqXpx1JRu83xpbtelbqO9lzzikLYKMsTGg%2F6Y2xQGFgjQT3iqQn2AYknqnl8PaYvVoE%2BkHsyex%2BmkJPA%2FYdOEoC2UhxxWf8XiYieWEGjD2szky0nv0%2BC0pwBjAGTbxa%2BIYycttzNnEjkKDVmNd03572i2pwy7xY85jZ9jDsiM%2FIBjqkAYyUKK3fgRVy1qg6CixkIL564gfp4UsDmUff4fQA6JiiG6X07EgDBFqIjgdwe9M0YsO0zV8hflthbqZ%2FG8Z0b%2FUWGsdKGjqNLVMVOG3xErwaAig6FyXsJcnv7%2BWZh1sn5ylpqIu%2BUdhLL4P0RCD6FhPj1DN7ueaSRy5XhpVFlC3H4jSIHikGwqDq656BHlx2B7cgGEal72dHOLNfNcAIM2Y8JTK0&X-Amz-Signature=7085900f85d34b7b036ad2aa39ef7d9593305cc9f5b8a2aa944b90980312cb0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

