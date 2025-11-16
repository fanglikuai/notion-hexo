---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRKQVSGT%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHD9LzrOvtwcHQbZS%2BostJxLiRkTNpf3mIxA8VySOOU7AiEAgP4ywo7a8VTJ0lOwgEYbr2eLeZcfnb4FKbzmozyUeyIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK5TNSUCYhmeEKFkWSrcA%2BpzANLiYp%2B2hMa5SbNHHrn4AaG2PDFdkkDl54E90Cfb5oNaCmwGe4f6HsGsC9kK7KTg9t%2Fif2VA7U8LuSGoqN2xYsVKVZbIOPJ5zERwR3%2F9A01GJpeo1TIO5MxKgHl22WL6G%2FNsiFTdvs5xFBSTEzsKqjITwk2whI%2FkvNIYw3Qr97wqIu28EKF%2B81kOdgTG2BR%2FysVHU8eoGyCq%2BICoqFeNmRGzP3U3BYisVz9QLA1iWKTqZxuFDy5duAIVV8LJciUm5zTBbcSFDQqOcDFcCn2MmQyxexVo%2FOnfdKRE2ldjp8HpR28KARlugmaze37bvOh13Ow0gFtPF%2FOZYBFYC04vRnlNJWUQWPOkm3jZzQdGcMg5LLS00%2BoOanAI1LIsMqkm%2FnWuwj6AD9dJTrhq9HmWXYX0Mb3fSeBMCwHrYtFxOXJpFWAuNBNQtVdORB%2FCMVNK%2FMQdz%2FbYfE9q%2FbUcjZCaxG39%2BZzyaIlHsclex3gvlnJbgk1%2F3hVJoJZFxXWBGosb6c5ZNHC%2BitXwboBvhz1jVbVcWpkWp0dXQPzFYkc9thU%2BnCvVaKxfC2wqHzH21EVUd%2Fc3%2BpEn8fiWOFRa7HVxw5F4gF6UVlFZcaqgFuixqp5dHcZKxAErg52JMMP95cgGOqUBdmSweKo9JGsYjC7dlT9tfMg9F7MUGmRTfPBPAEse21F%2BovEh7zjSUkfmOevKLAD9zeJiYKLFbgCgL9HGGwEmartGe3IomeYq9EIQLH4tcIGuScKzOwQyOQMJ%2FJmzLFKKXjjT07sddjorIgIlE9rMsmmESkuBcvHafAwHkCpNcGl%2F6EyEMsDOAzvo3HUrWylx%2BSDXpOR2%2BpfaYga%2FHufQZv8pdNY8&X-Amz-Signature=d8b16909a44afa616da561e9dae7c3f89d79dc336f9fa484f4b63fc02dbdc660&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

