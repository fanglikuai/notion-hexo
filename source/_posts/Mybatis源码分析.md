---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCKJEAY5%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIHzjd7A7H0wO8S5Jgi0Q0Z6og3uvQKavfEWUnybZc1QsAiAQNIdZ53izto2kpI%2Fnxqh4CaX%2BvLFBcxocwJuu6DV%2Byir%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMuDm7JaII46DoJ54UKtwDOu4JA%2FlxDlSa1lvO5Cg6dcVrlRRYW0Xu5U9UFQ74CC8O9ezwg%2FTgbJCIZRcEP5hOg6LB7z6mPInVGkvkDpntujAZZCDvPzYUWQcjhOrG6izxtx7xpmBrT%2FlHFHpqZAkzl4KIg47lKhCUAXSXU4q%2FW5fqMj8HMvT1gvriE0gvFgqkNhqvSqbA3yJZbE8nze1enaOBFhOh8LjvF8Ne90zzUxwn6eOZtqdVCzhDKNXQ8LTtlOsDYw7Ioa3ZL0QtZv8BRN6EQR8n1moqHSstKhRVvRK4yPy1XRGtFlD399EIMZ5A6dQl4adIGgK7jYef74EYR%2Byivb8prbiKiEYDBt7sc67PRs5hoCwxI3YMNKRJ1cxM8aTFUBdHN4qzWbqqS%2B8hCiVvHfyB73nD7qky62lRw4DBs92EivO4lrwp31gw4NaiWRV9lD6OMSRLw4f509HcTrA2CgWA7aW%2Fm44QLMuwotI%2BQJ1clKjD6QrQowvXYK1qDSoWkGlq0wE%2Fqo%2B1%2Fljm7bZEu2i0bj1s8cl3JEtbatteqhwXrjybRdi08Vr86TQsLye5ivZ8kAYEF171tU3LqtmX%2FldRWx%2FblLlcHs%2BbzvdjCMRHePW9REgbe7qJ%2Bl2OhEddHxrSh4%2F6ZNkwoOiVyAY6pgERXxYmf%2Bz0yz7xJ1HNtJwleFZHnqg9sYUappCR6BLPRRFGPa5FKvJOViYOBpYVGv4WItrugYRLlWMqedktTcVS7hmfE7povpUTcYfrB%2Frneaip9aeVz498ZMWPA0CdTXLtQw74ctuWLlRZQno5kRS3hFaMqzEQxmym%2Fq041bdLkMf9oK3dONCqN51lbrvnBia2JQNnfPAhYmoK85E5396K%2B1tn%2Fbkk&X-Amz-Signature=a9620e0db3cdd776d7a91c7466636f546ef6fb2c3e7a1a1379f17f9baccb758f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

