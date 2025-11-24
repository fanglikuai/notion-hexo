---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZRUSCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T220113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICcyaYecxAEBNnyBy738c0Je4e8rQ%2BrbOe2IXEkvmexvAiEAv3GJecZH4C%2Bbz8zP42aaIhkUFPJCMjewi60Ua8G5Obwq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDDCsqYNVvafZxLkRDSrcA2vBaxZFaoHa4bWAUOFAfe%2BE94CjJy8oy6UATHxD15TFiCs1hG%2FkDyhFMGxQOEk1KBSlTMdiyQyiZ6A0wMClRSW8zjVCK1GygXjLrciOe68LO9L2Fx09vmmWenmjf1B2HqBaUziPJsSkZiSFHHCRyjMuSsRRPl2MCRu1Yqw4W1LfNScn5RBdk91sjoLzSpIoR%2FWIwmkjleqYXLNSRvEqbasp9KsqIc6fZ0acBgBLkNIH7MJbppy9SkUFRvQIFZ2B2qhCHDYy4hYJNVMqGzgMI1LYk11MiNXGwvJyJ9pGyE5RGH3rlcxzMKjX1uq7fMDQFHA88OWT5cIWrkQ%2BpReLXNTyX%2BtlaeElG9neo%2BRh3cq0G0C4%2BBlNFWTgTk9CtroNW2DjUW%2Br7BxkZ5fysoCEzJG6R%2FLUy20u50R8PgLjLpiTEmnKURMpi%2BtrGSwnPQIjy4r3T7LruvPZAHD9qGOdlzSiyiGG39Y1SMSGD6B%2FdlkgqzD0PnR1YR3br8IHX1xdUnnBIGx9NY%2FQUA3jft%2Fd2riHMHCrL7kufOjd7pa6uXS%2BPdYoEbkucxfMvjW7RXhaZLElKS18Y4kTLWql715Wt1iuTNeyTqBF1fVDkXamIt%2FMIHPU%2Bi47bIzOxoO%2BMLuUk8kGOqUB9fNc8ZXZjQMFEdXb2hYcWxCpQVNtH7pAVYxBdwi77kyRp0oDS0%2Fj6CtgLepCPnKOZMyPf5XHS8ZIt7AJxJfPODRfITNXfS4jPsxK97HwB9dCqmlfymusThjGzzyAYU0C41mNfIOZZ6EijY%2FdzdIJ0CbzfBABQAZAJJk8ppjcqsRWqpXxg6UbnxEvbqvd8y5CrqEFs5GFQp02eokrXIuW5j1FKA17&X-Amz-Signature=be4e6ce5a21b3053cec5704a1923e600f69abbb3e5ebcbdabe8a1db446545cba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

