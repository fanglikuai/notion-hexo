---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZSAI7W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCzpXhKLqDIVtgOCiB43tZ5X6RgNVUFURth%2Bz3ZGQQ5ZwIhAO%2FsAxzyac6Qov2IdVj0HEVAwzmOHyiGFxKd%2B0XWA%2FpbKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzRXZWGbMSF4yOg2Vwq3AMNpfOagOSQ6dmWfn1oEuhDNvZm9w8mjovdBz96fY55gd7GH64oXyMJuLsznjq0X6W5AXlPRQSUrSPbja3zU8pnP%2F0T%2B7ewZSZMU1mE%2BGqoioc1Yryo1cz3dyl9rPZCDpirkrZg9ZcCdRMx2zLU4OLOORY8NBvDNpo887SQ5KLkpWKMf27U%2FDWlZvJkCFlD8GIpg6UTERLzjx04wcCtj7kbXfwIs3am0Z48M5nyJ3ZkY9uWYzNKSGRlH3McH9k7ROUyckHudThyVKLgZiyu8vr1Z0RqtLkPLVyroglAdL9k%2FPWOFVV1%2BjEAw0xDTFb3fRl7QAwUgIvEl%2Brth4YqyfCSCgZfgyxwWmF4S9Itdj9cYb4GbC8f31W6CRnTQuRl%2B3M6iv550D%2FTF%2FTGY8peH63cxvEPQA5FztOcO1Kyk8uZCXMb%2B7cdjkbAEyPQeuSvy0N7g5Dz9xkDm%2F0NZL6H5K0200mjqpfEgEh06A%2Bf6YwkxbjMN62AWPsaEulRBxEBAsnd807g9F%2FsKFl7lZ0ccdYJ0YJzr8Zm%2FtT1mUHRd%2BNR1v2qlGxoR5IeRYtGarVSQagzAFUw8%2FyDa7ZWOq6rkZELQIYUv2A%2B%2FVk2zzEgXuy9v8PbPLbyLEx8WJjOSTDq477IBjqkATqq2eteBGMSGenmQUj%2Fh8yblU3N5CpVlePSIkQ0pTw1OHLcW%2FpHfsTlVv1ldFie9zCSPgmovCMCYYI7O8ES%2B8ncnONlgR02fJnA20l00A%2BF7TzwmdYlyeCBFcXT0SOSLqkH7X%2FBuUaKIJqjVVLGTt9Jm8RAD39L%2BKRHO80HB9fVcpCShv%2BLWfwDFltsQaUlAKd4qsZHBjLxQk2r%2Fgynx7c8I2qF&X-Amz-Signature=fcfa49bc7bf32545928a9d7ff6e1411e531933e33aced61d3e1016adca923d54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

