---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLB5WPXX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWfzqVUO66VOOeXKaqvl%2FgN%2BpQceilbZyFHRbRG3IeKwIhAN22BomkSBze%2BSVQJGNyQ%2FefCW1Nxhho3V6mAqvoCx6MKv8DCHkQABoMNjM3NDIzMTgzODA1Igy9MVx7jDATHYsfquYq3AM0tdQn2xmgsNieSwHcT8SJyf1uaQCzm1iRmD0vok0mw%2FM6aUVsxKVw1RPpvYzFB8uuAdB4Klett8xvhNyqqihWlkna8dz3DpW60%2B7NvDFubC6fUZIMcCoC6xqjs2bmr6C2JhxPy9nXt3K4JbU6tuy14ufW0mq7Z9whCzNQ1XKdsm6LjkUsqFdW93r6rk3KEurFyuE8V6%2FMDYosZEwoBIGMCScWcxCodfaeBZHGjp4o5g%2BTXUuW9Acq%2BoKSCe7qtChH9hn2HtLOh0NPakFDCt6njQeP1ecdA4TZIvXnYjLJ%2BpCm58GhVfzroWb4ucv7yi9a%2FRVqMWv07YdEnugEO6dTgAQMxjoVDFtsaCZp6IUo2PzK8pKr%2F%2BMXslJLZwOhAkluHkyF1C02pnF5JDOCP%2Fktr1YrnEP4vg8tgwOUskGcRSDy0o8%2B52Mjuz3qR7JNyCiNw7LbmoV1w8jiUstxJkPU0F3f%2FCzrRhhz9FECamWHVTo1lFI4wmen4vmys%2BRZ7OmOpjRjqqqKeqbDp%2Frt2nVfLj7DCpSi1vJfWvcdEuINjVuGSKX1ACAI5KVufOMMNhZcEloSKnKni63dGrlYRCJFcpc%2FhSRnmylp4f00utCqIH0E47WOrqNTHQMB0jCt8vPHBjqkARzTSOa2qQMzPFWL0WQoqAvIVR%2F0vsTo473glfoOopA0h6fIerT5kWfAy%2BjVuS30B2aVJ4QC49i8UAL6uQtNwI6bRwAWpOaXFQkbSV2zsJXzI0p5WnL%2FUgHC23vTPGml1os977mtEb%2FYuFC8CP73LPCy8f%2Bm995YdNAUlji%2Fr1pJIx9pAt%2Fq61ZIci7dJkorTxZsZzRcfPLFmVWpcFw%2BnOPKmcTk&X-Amz-Signature=91e5d0399964d505d5c51863fd676d1a0f6b9ca3a7d5b8e0fe9d495033d0e360&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

