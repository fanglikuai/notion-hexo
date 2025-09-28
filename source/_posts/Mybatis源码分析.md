---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKX3CXW6%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCG6ul5T12a14Th0wSLyT0Xk3up0YU%2FKTzHLtd9gRp%2BDQIgOvF1A66UgqY%2F1H7R0s9J4hs75Eb84khDDK8U%2FEZR7W4qiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2Fn4plqcrs1AgqwQCrcA44Ukzrzscb7UWtjRF13RmoYL%2Fw1KQL8%2B6Cb7ztHK4A1nvo%2B15CGs91XYgs%2F2ZKtxX6gw705q%2BzBfjEAe0wsB2lCB9q8r5DMCYRq2Z%2BFLtnlcEogX67w0Kh45mP4RKHPvVC2VjYMzVGMJcj9JWuedshnxHcNXuGgErcaCZVtD7%2F1%2Fs2Rcc3iktG%2Bli7Z65xfhugwMhFACSCwLBlvJ5klcXU6ZmAmcYw3CSWBZQzaRMlyUYKy%2BGsGbLB6hDS8W%2FHYjC4Eaq9FrhZv%2BWrEyaA6QPW27EvK7WrWiVKrkxrrZe4fuJy9bVNgkBUM%2BclkvA19ditYx5S0X8cwFq6hnh5WOhrdz9aVvYi6S3y0b%2BuvFooH2hFY4jW%2BpyYUnHDPWAZ%2FUBGS1JzjW3jz5R3Kvc6%2BeeAqjkHihi32i2VJnH106HOEAVPXPUdBjlacvErlu2YHv83LDmLlZabLwwtwIA5aQvUrsrrax4OdxJEDByePO1KKziZ2OxTSm0EpSPqfoULRQDudpfpiVUGvehYDYrtLz2tCzrtYCPynT%2B09FZxGcInHOnv%2FPn4J%2B8Oc3OTIlOw8jkdHObXNeRjkUWT3oArCITZW1I847%2BTzVW3akIVg0cwVosvdtyeHhdGhUO4UMN2a4sYGOqUB1eITAJrYW%2Frez8vMCwwlu5M76Pw30A%2BSucIbS5Yfv9d6zlOCSj8U%2BNqsIOFe00Yu5%2BwP5dXP%2B9%2BlfaHgzGRWOHSnFlVwHOQKjYhRkWki%2BytkThfyb40EmJe0v%2FYIMfC0O8w%2BW2YAAelWYDpgYUad8OWRvAA%2Fdf1afxlOdzgUF70l2cGY1JCC3OD3aTKFhGnYFjw2gGKIRW6RKwO1QoygAX9N451X&X-Amz-Signature=38d8f381d80e689be3486ea0a399153750b1e6da0a1ee1e3a662263d7a54c972&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

