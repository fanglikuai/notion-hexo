---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WXQW7MI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T160110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGnSbvBMxsZTk2NHSVLzjmqWq9Y8jvBMUQJhOcxXxEZdAiASx54lX5PTyaKmnNe2sJWKtF26MA7VQ%2BOqHvsmRas%2F7CqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIvJQlyKFHdCHme1EKtwDG5Z0MhttGPMfCoIKBRb4iKfPE9MSteQq4jSkmt%2FvtvEIh3GylW4KvKQcOsUnco4afE8vLywhdg10NTY11K48siRweN7DS%2BhhhNuZob%2F3GrQNEwYlcMo4j1aJmhswqMrwsonkXjL4bZO9kETcbfvJ5sSLiKdqx%2BgV0KtvPkVIGsflhJdk3htfK2pDcTsnqUhcTt%2BwxxkXT0F7TaDKTCMzKRK3ACQ3PMkITDqil5jixLKRpzyni6z1wB6tsh97LjR3I1OQ0VoXDP7qfuIMGU%2BnyqfgEeRiUqP2kVNi4mYxkU%2F3uSuvl8ukd68vTp1G4iAFHZykGpVtXi90MvwY3hfbB7e1sxsKf82xcegXZNfaFkfDmAUhBJaCvR3HW2oDrH8Z2Ds66UvKp2DAlT5xXms9YtBOnAzSK6e%2BKwhNCfCGsW8USQqjOxew9Qzd%2BUvT8u%2Bz%2Fon7Ab7f6fa2yGtgQyJMNooql2Zs%2Bb0JGB2ITSata%2B6WYk3HKlHhgjMOICNTp3ix6L2zLn1PPzPv%2FWFddOzcD8QB45TTcnpYJOsoRQqU0YgkBC2RTkEwgQ5SoruXK9Qb7AhXU0%2Fi1Pu0%2F5dckZHZ5n%2BelVhdMgOkIIQQJBidaZuSWs%2Bse9F3LpnzaCow87K9yAY6pgG9VXEslSKwrGzvi3gfQVPZAupNo9AMuVFNUqTntsnX454C6cJ2FPZuAa0xARjEvrusA3%2Bsz4JIyrhrUc0TXAZ3lfGtWaLO81BQz5yQ8JHSXVV55z9IQAnuNAiNMLfYSLPyqCTfKxNUh04PIrepzCkk3M2TnmkDdpNE1EdDDoohbk0wxUGMi6GtZBdOzwcX%2FLoEOx%2Fl8YzH9BDYWaGsJz4OuCRfw0T6&X-Amz-Signature=bb8d7e4b02b1f49b4ca7a6e31851fe68017fa1cc4e5252aa85862f71770401bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

