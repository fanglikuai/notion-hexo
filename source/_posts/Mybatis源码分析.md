---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JHTIPWB%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJGMEQCIGNmbiQLzYW2PoidPiJppT6MS3s3ImAPD9OTyIxo102iAiAfV7KoHBQnfuCdoRcpFyfTzIQU78KK4BlljfL5OiW68Cr%2FAwgxEAAaDDYzNzQyMzE4MzgwNSIML%2B4Drda%2FDrUOb%2BYYKtwDmirQ%2FewNzCoq29VKUZV9IYty2K5FqG6IMr%2Fz7biksCEAw2mLGgbRZzVMyMGTZUeM1Q7dzrr78JuRV1WraZLwKf%2BKU6G8qjFLIURh0iutLgo2ErGJSmEtDryn2Tim6t5xtkGEO8jiiCNhu4i6ga%2FXSO71pZ8ltZUAQbrj0kmlE6majUiCJitx%2Bo5H13TKnnFSeiUev6MwpeuGs%2FrETtKilwPZbw81oUOHipGSnyEViyINc1i2FsEwyTufMIiWzWWnQbo%2FrLRq9K4ayfFLbY9PbzozV5tIgHpNcFWlp%2BOoBvYzZJhyg5pq4iIuR%2FnD1Hulfg1giq8Z3JV0nqDU7qX9bUFyZgTIL6lVH4AqlXAkRdJZqBEze3ISI%2FZr0HKIqi1J2zqbnGZ74TdHNHuN8HLiQJc5MRu4v3Lo%2BQeFZ2lIx2XRUNGm%2Bn2yqhN5%2FBnhFqreq6djcSkB9XdVzIixm5PXi1NOSFuJy5AWHk5JUrcHBc1M5%2BmmXjhxtKp0nvRhpc67uwRLKFeeNEF%2BBbvUvcPMUGDZOerKn1RHZ%2BFmQllJ8gZOncn0bAIt%2BGJYp5fjSnksFxJpZmiQvZBtAkfyugn%2FRNKg5c2P96jRZiGlQSH4aMdz3VDoPRMh3AKz%2FqYw8fTjxwY6pgHb6%2Fmq2MZbQ9U14Lvu1fXSb5KkbyBmScxjbD%2BuHyVTh8XsLrstMzurzbrWKOQYPanVy1z7Y%2BhALZcpAkSWLESuBQNA0VK%2FuHmi%2BOWt7nvdsXQx1Q0F0GUBS9URwArCHLhb7AM%2F7a%2BkT8KG5Jda%2BH3LZXKczLVymSHm552gi0iZfKwGqhGqedxb4XE%2B%2BVD587AYHnhPkevqUCPeF%2Fo3ahW0n%2BdL2gJq&X-Amz-Signature=5eb625f1c9f187e955826ec255940f9de6e1223800954896f403da6d509feeb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

