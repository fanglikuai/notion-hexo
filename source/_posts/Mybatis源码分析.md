---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2HOQBTT%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCoVKojA0Oe9DAFX42X%2FS9uRs1A0QsjcyZ5kO43b9PPZgIgVC11VNitWbRi4QTZ03tyitghxRJJ0ZyxlKvSh8FDc3gq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDATBwefinMClWa5r%2FyrcA%2FAyVGOyr7VI38JjLdP8J%2BeaNkjhPPjHgsVE0TjrOPvGoIDRaTVRt6KSRFwajkqjcsL7Y7qBwSnQYgo%2Bsp3OLM0DlLjYKzbPU6BQyyDBrDi3E8LzLl2tLb1gJrClh2o4bAvMIMTq0R10UtZlNr0rP0KOEVFVHPkW3TlTtoDz5NMZZ%2BrvbyPoynXLtVR8nXeTQTYGp5KnlO5Uyt98bz0oAwi5lI8LFxsKMopfP10hIjf7tN0ZSaHhbFnBCZgC1QNYTZBA4WyrL%2Bm0GyLvhZZRqSt3pv%2FWRhG7a8zpBYbv9ELjF1Xn0zIcumHpD8oG0RqnUSWOxv4RKS%2FBn2lx0xjJr%2FbbJD5QbMNbApddUlzzKp3Dy5D0avujXqjRbb3NsJyRXepQaIWd8OtdIrzpXziR2T9YYmQpPD8QIFJZqb85cu9ntPFMo9RoR5LShxu6gulzuq84bVmrdEo9h7mhXiVhlkBa9N3l5lcVitybNoM%2BQQpVpxyD9DNb6c1WA54gw9ssTbVgq0TRSeqpBav%2BGCTD32mDTIVov9K%2B2DFlKpYnULTuWIux4HaIsbH3hYlPyB81VVDc11dHVTuvzjaevnalYgJfFJ6efK829VG41ZVQvcR%2FPbGkJTSTgh%2Bdk%2BsIMP6ghskGOqUBbNmI8Ud%2F9nIK7ohMKaKZgOSzV84J4ARNEztyYj7U8GAxpMcG9C0LJWnSuZ8UXCbBRvPHdKmR71o54RM7D%2FydCQruvUap4TpQ5dlhujcraui%2F5NYQ6qwHb1IfhZsQtkanI5uNdC1tTEJTy9A8eTnZn8lPhmPB1ZjW8M0EM9UnC8QbA7RHPfoYUK%2BaPwyylz3ocDUIbSvnIj4KdGSNkQt%2B6RSaWoPi&X-Amz-Signature=b94c99534567d06c49d5739246bb0a019b938ea29019c64fafe56296ffc37ec8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

