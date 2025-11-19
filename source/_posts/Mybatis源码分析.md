---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QDLIUU2%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIQCjlvxH9dVl%2F1VFFdzguSV1s7vMTJyeghsdnYWC1XaaGQIgELaIdV3lPapDJbEtrOqMkuLnmNGKN5K5dFLwG6Vghw0qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIkGeo4NzfINjkLDfyrcAx6DBk%2B%2FMgSnYja%2Bs181GL1fLre9OnHTRNrmjFAp9eQsk3TYG6Eqq9ASr5Y%2FWsFO5VGkYSZrIbxzqgdDhp%2FM%2BFCeKQZoHX593PhWubQkJay4AU7M1WVL6Oo6OQyoL%2BBwpTMQ%2Fu3yJnxg9Xa7A0295HZCPzGshyuUC1ErSVQjZVlF3tNmlDrQ1ILE6NCE0OryiPxVKJkh1RYx%2BTt59tN3JPdJTDPrUKaPueIfIU1MuxFMLNYVXsxydupSQvA4g%2Bh2%2F%2F7tzPkryKa8b0GU5kVWamXyYovMyHDfITyb%2FWy68G0lS4a4oUWIpl0oUqBaNmJdC2yqBxBAGw8X%2Fi31f7c%2BxZAaj59w4SSpwe7DChbZuGp5aO3vnpFL7jXN3YI4reKvXHp8c6ccr44S8FIwTy7Y8SCy1BkCQWBaLM2XrESvwTH7sOnVqlFgTnRLt4pi7zBZBgbsuyBLvjIT0zeFd6PUhY7iu0FvPbnA7hukfJaUs0a8TiM047181Z482hUfOZyeoxPZd%2BFglGRqwRe07ge33QNdbh0EyOM1vvD08fi8x3aPzqcOKcYniXbNhxnXsTVjbdayxJSBwq35vRlZR5w1y3yIEnDcWv581N5o5ZsdLKV8RUeWGOBrjGZSHY6HMOT788gGOqUBeEsqPpe%2BPPuSBK%2FhEszQjAgsyrtwDRkVGCeizFfvcHcB3W3REO5U1PJmu5aBRFCbYQ985P%2FaxNbtFT6hY2QNc2FPVeAFBI9n2%2BGnIr12JhwPdS8SILqow%2BlzMe8olqRwe1hH5uovph2iU%2Fe4EO3BQgi0FDyI7eTtzT%2FloqURXDLEValZ00u97VAY7crjnqK5MRyFAcTJ%2FvuEt3gcrsgH8by7A0sL&X-Amz-Signature=9cd268063f65975cda7c51b3eff2593277178fa5da363fc4a4a7424f0bca5999&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

