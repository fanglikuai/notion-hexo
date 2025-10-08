---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3YA7BDQ%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJIMEYCIQDPK9BC1e5Oh4LjRiIUZNHDv6HpAnw3yQleor5%2BCvj3JQIhAIo1WreiJbmHOvs4o%2FoKHd7TUFzPr21%2B2RoUn7N2TtpHKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzeZODFcmMi61jfJeoq3APasbEqmP8GmUkWF%2F%2BxkJt6SvWStWp2YzSHXH596iIJIrTBjVjnVFKGzK2xiiHjjGGb8%2BLbAgDxZAWqFxIqWtDXRMiV0LfdbcqZcH2u4j2v4SRvvWXzKuu3P25ar%2F736xd3bmpJs%2F1xz9jbPhgJdbmP8F%2FqL9xjtFHR79JB51Vv3MQGVcVVEetiywTQVPmB1WIEwUbynzygK8K6ITuBV4X5ddNAIZpi2e7VJAbuL4irghd5DNxklf5DTdo3TVr52wW988fqM539uiUcAbmAN1DbKDnl%2F%2F76PYzaoQ7TrhbzWX5d%2BSRiFMzJ6yi8WPf9x5B2Z1lTVlCOINhXj51WX1PKud4zhzcT5%2BhkkiH9FkagCBNDbNuflU7ZmWmIrF8FVf2ED3FaXMSzOR5jnSueoe333GYMQVdTFwjRp%2FvwmAbuFSl%2FpAsD%2F9X8Dbzy141ROufJOvC9m0cERJJokCS%2BiuyfandIiZAK5aIEcEi3qVcRLfgpqGOi4IJL5cBKRYDXHtE6Yf7hnQuuAWQTr5wLFpJCRp4gVt6KtqiMrJA7SIuouRhNKQ5QB83pXEdRxtcNH2ti8VWKEjTNvxDJlte07K%2Bb%2FEJVK0izhaJURcE%2BcfVsZoYApfv8seOKmrUzOTDOnJrHBjqkAad37dy894Ixpc4zihwkT5V6owvDreaGkeHI3RCzW%2Bnphd%2FtIPyCyQHpOsGr0Pa0eFB1niIQdYLtk4r9XLqakqCLRILgjXa28gAyheQ35yrbLoWBU1yFcT1%2FWPNDKDYNgQR0zlBNlqeC8z5qRXoIU54kyyFhJGLGUFcS1nKMzQR9tgGludafwN%2BuPC2SnTbfesSvNVpu0UEX6xN5XIt3hiTpbz15&X-Amz-Signature=40cdc648392840d90bc727e4a8553d42ab54e6ad38efa19e9d322111b49f58b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

