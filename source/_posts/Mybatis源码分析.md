---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXQLL6Y7%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDb2a5pq%2BIfxn7SL73JOG8H7xrmtEMpNpbVSQf2thIxqgIhAIHKPZgNJPeOguC4ZpUgmOdsleGu9vz%2BK9mRNnaSVt7wKv8DCCQQABoMNjM3NDIzMTgzODA1IgyDUourK5ItO74aAxkq3ANbcmlLZ3U3lUom5n3%2Fi7b7z4PQKESc%2BDOWI9nu4SNrzDoOzYJHbxu2h1ydcoOIrfLPWbQtUSiUUET41Mt5BkzDnVTCqXhimanOoMcUFfDXWN2j4swBPY6%2FNoAVYdGmkGXORjCdpx%2BBxcyKeHIMdWVVfRxYOIZpLQqrv6XdIZ4alZn5mwci0DgolSIm5pyxzZ2O%2FZ4cjLjmZ31DNsrtrIvnwuTtIrmhIfXXNwz%2BP3wtnxJtj9GN6LQazukO6EYLGM7S9CWkE6RZ4SHEyJPSgSJRTcZt71eZqpGW%2FBGd1TsHTNrcVju1VmKmds9xcfM0GJosWeO%2FefT3iKKHckL99v%2BvP0bIOEk71Q8QUzsYHX806yf2O9s6vva2EYpcOmYNJZxukRNG3Pk5dKNdEme23qRwu%2FCWBpqgF%2FNDuYH0ZNdUVRp5YjeeRBlokcPHU%2BTWZ6SfCWvWHJSKquGwj4Jh%2FboXgIml5MYn3c4cNQwHwIWEH7VKZP6ic81oHEWvi5%2BHVBcCM%2FVpw0oRb6sny2BZUo1eNaUlXQjmqrHs0KlPvAesjydsW%2BDQqCwDvZ3tXLEtBuke9Ykr1o3jfM2kV6CrGyjBo4SlS8pDbOlUe2dWVwh4rLKhrRd1PhNrVEiTbjCKoobJBjqkAcBQTlaeX3U4hl6ieql11svCUF%2FAbZj4f7XOhyKvb0QsUrbKSJpF39g6rhWGdbV1uiNhNIWtQdXqzMf6zSjbSKl0Vv%2F%2B%2Fl9sigp8aVsod9HSm0kPGG%2FOM9qjwuEkxP3lifg%2BDJ8%2BxE42shI0uuCRRT2GosgBnbBnE%2FqA4X09SdJGXUmp4cjeC2TXM0erhV3naBd%2FpVWHpcrvypKO7BzenowV3Qx0&X-Amz-Signature=21a21760c8c165e6560781dfe3f7489e8e05ab832e48032b67d163fa094ca3fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

