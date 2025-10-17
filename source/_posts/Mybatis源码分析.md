---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J4FDKHB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T180056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIGj%2F3w9t%2F5nMieRMOILcj7c%2Fs5UjUFYmS18AB1%2FuiwI1AiA44xyBkeBkX0IaRBMeuVHZC6qM21HYbydCrk%2FI32gLXiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FLOc%2BYsiUTURiUoBKtwD5j1OuE5dtwBSZZKOtwkTS8nbQCQIP9AJNxA7WsTqtxgKva7n2QbFwHv8oZkq0gYPZWf19LhMfKDuL69qvS9WWbXP80bcaK30feFRlc%2FT9djg7OJyYmWR%2FYFUqSA7qs7rQXS%2FEvcVwOePqFk9Sk6qBBsXmYWq2E00O9B1njCAQrvvc%2BA2npsHFsJgvc8V3lTWUzJO1bG8qsCDuxHaqcYOGdTmJMny9mgqAEakpPIaWAXG3Jkng9FjBfBRd1Tt1TmkagXEKcxNZG8JymrEE0HI04a3tzGRwnIR8kwBjQCs%2BNinUNPXlT1xFhoGv8LFurAjgW6dDS0d7e5c9Qvxu2n1e1qIPhnuD%2BLrBwb4K%2FsqzBCkzKj2%2BbH%2B%2FUMoKOPGGtjErO5huVvpScsRurDIpMNpLlzm9C8%2Bxng37siAmBdVg1OouYWl8M24vwe6B1z3wWWRxAI%2Bjw32JHtZKFmUglAhNcGzXnZoGBPtaPbBHysASTZ1vSROtarJ3h63JQF7HcUv3QvJdhozYgywqmV%2FWnPkgIji9XYh%2F1%2BIT%2BjAsK2Qqb90Fai4XoPERSQoKVYeHHt0lsws5iwWcaciYPytwgehMkaPxWfC6bVLlU6vaMkLtciKYDFCd05TYu8s%2B48wyfzJxwY6pgEXcbSHy56bxLCpbrCxhLtES5bxlx6VWawDMdS5X6nMtK9NHQS9kVxE2j4rbdfM7iUIWVaW5%2BGjrWQ%2FIhAXdyg48HD3XR75LC8smY%2FLFJj3haYB7G0jJ6WzHLpFeQaC53QwaR9V22p9f68ysp6Y5xfHz0ZiqFnwh5koHjStGbuTGNGPAxFrt5ykSKTBkX7o1t59mnOONef5l197WpHH8Yx8dmQ2gQGW&X-Amz-Signature=386690b440a03a3c845eb5e8a6d2b4b664a7ea6e29c4d2df17ebbaaecb2be83e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

