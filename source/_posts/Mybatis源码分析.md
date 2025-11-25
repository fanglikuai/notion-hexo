---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC6JIBKL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T050106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZcgsKtX8bnSiS5ldOx3kuZo6BB8ycu2yGz35qAv3POgIhAKyNtWlhbjBy7J9feJoZDilFQ8Kbe0wAeHL%2FOYxJAiTXKv8DCGUQABoMNjM3NDIzMTgzODA1IgzBaEmSKKn367GzmOYq3AM2gloLkjn646PUild3OlDkSB2GTo2TyovmtKl2uxmQNCF%2FBXsFjme%2Bu3eFpR0OAhfWEfbqnjDsSVzynRc58slCLbhJxXgMxIA9ro0tepCd4HmQgep5m95SCP9xE7O%2BPKV4bA0d%2F7tjj6eptyMLpdO3YhO4HMM38%2FGmEflZn8kx%2F%2FhSS73wsvAB8lfF6ukC4eSiA2yhQqjA3KJaMehBD9wmW33nUt4la0%2BekU01hTrS3ttDahJYdS15K5fmMzLJ3jM0K8r4tfGVgnS9uDBeuXq5yijnvTrI1sA1StE%2BVshiJjP9TFnxTRWqvQAre7lLFQ3GJyMZf6Ro5rapn%2BfFvVZZchS%2B6fkP3sB2dYCY9jIJl9gZHhbrCNgXX4CdA%2F7nLljvcQcNVXtqenUPjqq3pU8LQN0eGmLeal9rkFlPsko8K4jy09t2aj%2Fw321tVUjajU993Las2C8KGBsfNSTijXFCCKM7qfWUoXpBxApBeAIwf5R4SnbCCvX%2FtPnqfeboBKPFZv8h%2Br8IqakE1byAkHmWdEP4Bs%2BtiiLFkpdUwO%2BU6F3oL4qyMFCDg3hh9dezfRr5CUtS6m02o5obIq6XyL%2FFNrrgYpxAM4cFC2Jk9CgCCgzeKKWgIl0wXAOwRDDi2pTJBjqkAYYWS%2FlXtvAMAowMRZEgp1FIhy1UTlFPHdT9euhoi72pJ%2BfjASB6YNlBehLjvEMJAKxPjLHQTWeU%2FfjCbP6ml7608LSOK9mhSJ4gQdaEkVGRHvUxuH98nxq55OA1zJik8TYpEFzEDNAkBnVjNLKFUoBavflO4wRtEZjM4RNYS37%2FqM6txGPrIj95Np91pVoMKf4eDbsS8ZcKWXck0PkbbqzpsFkE&X-Amz-Signature=425baab7afa11247e27c3ec23dbf367fb03f2f524768177e8d5927d13ac3fbbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

