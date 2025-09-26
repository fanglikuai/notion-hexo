---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WK2RB5XT%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFxL%2Fon6Si5Uguus8vCc6hzW4Z7ie%2FWC8pcbeUSvT%2BroAiBV6aVPnqi5Gpw7mlXfgH8UGLZs2mbOg5gtwo%2FgZKp6KCqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZ5yIhs2L1CPAC4xYKtwDT6cMKpS%2Fxh%2FuzAv0uQIShMsmLDH3kRBMPOoNdlVjHZtKzWEQIiAwQulL5YZmlK629EjkuWR3%2BBNePoLtU5ucjaWtew2BwKXMV31hTpafj4NjQ0RiBkWlXJPONCzV%2F5tOhDaX7b4NRYxfH6K2hnfqVW%2BdsuCzrabsCkaPtgh8mWkeYbu77dQh2RQIo21Yuk%2BVUR7MQG%2FH0aIyBgohf50lSM%2FK1FhUfmPLgwOJ0RdrGLvzdC25LWb8IGspX1l2KCTwtcM7Xrq36phtrUVS0WjkxzDxs8mo0FymAUU6g%2FbVanU2CXxVYiu4JVClzuxZ6iK4GCMsNUSg6tcCR5u3u%2FuO1xaZXvegMY39t2QEpcQr8k6IJfWuIEtH%2F1wLV2mpbwVD%2BxUdIsKA2021y%2BOWKD31I1sjyp21TmAzByik%2BhsUYxfG1IHr7O9pZzzfikY09f80IONP17sM9GhUhgWlHdMEGTdxrlC9EeT2VylgtFij%2Bg5rJmArJZoeAshpAENOOROSomiTWJTcEHsHTFlfCFaGfABv7nhTUm6QySPIvfsF7DOk%2BikJLApM2Cr%2FXHq1YJEiPmIxv8n4AaEGt9rCesAEk8PkSoyo%2BCM1GuhMnpowFYMswcHS%2BIms%2Ftypd8IwjYDYxgY6pgFncrQP0f7rOw6RKMMwuGFA61iX%2B%2F5YaB1bICJ7FmFOmjBP3HohP%2FOAL%2FJ9VB5Jlamqg%2FvjwJGD5F23DZV%2F5WtfIKeGTYGgfiYX52AMcnhRx5TyX%2BNI8cSf03TH9IS4foiT1bYFy9HeO0eBCKuKRKk9EHdFZ5EbLdGZm0Iy887Rewv9xHt8bgESxgm7OiA7OkejC3sUy%2FyF5JgKxpOqfg1RxtpHRnbk&X-Amz-Signature=1d42d0f7560a49f3778944952eb5f1fb73d9de9b1acc8ec5ab5a38fcc084068e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

