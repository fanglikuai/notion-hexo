---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MIMEBE5%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIQDn4FRP8OrNS45ylXLTKcsr5eNFi6yHvzfWMheU89%2B1yAIgBP4IBoOpsfG7ghxXXcrKybCpbbB2O2NxlgwTUUou8lkqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAVNLZCP1wFg6VMGjyrcAzaM5HnUtGs%2BqBRTPQg1M153swGlclEZJiUm3vaDtRHl1Ki5C6aUia1kQNzqVAPSXWB9aGxcCfIE%2FvWbdjIX3buMFiObBGq2nqMIrWNHw40XJs8PwfrHHT%2FH4m43TYP2tiQ1dGEf4McMdclTektxWR0KeqAZHDlWltRae%2BYxIpQRI3HM1UgY693%2FeypOT56fq8Q6Z751rzkTf0QS88twANxBUfXtHN10QLz7LbqK3kCYmKjhXJTXnGnXPU3WmtVyRKOQcWb1wyRkVUMGPkcTFaF6rfNKSdRIaIJIaTycBgUwlSUm2GPmG%2FZwlMo48ajAI0RjMhQmbitDec94DIDq%2BeXVBtNxQD7XCNJlfG2YekxpfZmOXOSyDNqoRODTBIm7Ls1VTw3wHZZfg39REN4sVvj0zakhg2wxm0nt8JTAEsDzWMG6exc8RU4KP2%2Fd8G4Tbeii%2BfZAkYa1hBAdUIy1Z%2B6xuaSvxOkdTK1lkwjjj6Kv%2Fza30rMkANpxyccJNpzG8L8axqQy1VcET8etwuRAR%2BZ16dAV6e8ycZfVr17K3rkjoIj3cBTe9j1QTA6egxEDgXxGtEi7jNUwz3JI8q2ERtPqz2BA5bTtrQBwKAXz7lt4IUIb4uvIhqLkboHIMNKCzMcGOqUBuDtAzW0xx9MJlCsA1vJy0qCqbB1XBT10fPHAoOR50P5rilvXfNhCQGsz6hqiDvPzaphzywRxsNKS5pBSnx5RiSLrMJcPXuvTOeEY9LQ4sYv1H48j7ui%2FKqqZYNBBk1B%2F%2BidJF1Ezia4GsNqVxQxuq6p3oq7bBLZiV1aeQPBQiBGEA33rtVMTrijO2hbJW589y%2Fu%2BjSn8RE2SNT9EfzkvjOwS5oFK&X-Amz-Signature=0cc4295a8acfd27890eed476edcbbd1a33ee56d9fa049019369d981793f6112f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

