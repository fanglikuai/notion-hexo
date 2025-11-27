---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVDEQ3NP%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5CDOGyI4Xuo4mNFhgGn7KDls8miihkH1aXYLGYF62LQIgB52qVgHmdD%2BN2z5U83ZCCTmLoaTSfjp%2FvOCh49P8Y6MqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNg4XObdxUkc1uqj9CrcA7aU28Nww0vLtq%2B7JsvYj%2BC1RTl8A4U8JqpP5j8%2FQUtrg8mOj5m2ZNH2TG86Rl9xtr%2BbDmxP2qGwQ%2Bm4CFR0LT%2B%2BhhUzdS5DVFuTj8peMR%2BuBqj9K65Km%2F1xCKLJf%2FO1o2fxmHqLv0F9ZMBwWehTKtsCErOeJojEUYNyxYraV3EmhFl7UUYVep6pzXBvGOUGyv4eom67J%2FcXYFbY4NuPqT2JBB5gMjDB0bHh6Axw4QleB2%2Bd24w4vuxowpISmgsBjvnylluD%2BIFBOqSA15S4SWcm2DauQy%2BKfcUXoyIGSoi0LzxWYqS2qYxJJeBy76YOTyr%2Fn%2F7rWDeP4C8WIDXqESU5utGGDSNFQ1RkYbckabjwFnDADYwfQJK7YXkAV5%2Fb%2BjWLaC5F1xUR1Zl4jnDwllOzw2oZ6Y09MJyUnHiscNtuAvGta%2FTFaBNZWA1OFLow%2F51JM9e0wtcH%2F5iSARiMZftvw9PZkPf1sz6ecfbKB88iTUHvBQ9fMyF1Ojt25qd2rQOO7UvB23AC%2BlHOCS4af2mHCx5BbwnP%2B%2FcDKIRsD8cBsHz1IqKxqBffYlmGxB5L4wsEyuIglaS%2FafnIUyaD0lQvx3MT6gX1JB08vVRK60mM78BDrJO6Y1vD7gxuMOuVnskGOqUBqs9emzfPbYbNGYFwXg8cNl4rUd4plzmfeQBD9HidGTVmuV18TZb3e2VHtOCHs0Cm%2FBbJXH3YPRGy0%2ByN5CIoBsVW5KlHWqpmLFo9JASdOhrRV7wtl6RZTLHFPv%2FjrjpoTdNhcjrDvv3mv47WEnjmoWq7ZNM5elV6QlTefFnKFeg7kUbn0nFPPSvY3ktC3dZsnoXE5C0uBiXMCCQ84P1w7HMkiGAZ&X-Amz-Signature=262fbb63a6bb82322bb261dad25c9063bf12e15ce9c41a6ab56f518da877fb0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

