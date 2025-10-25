---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPUW5HD4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBbbx41dz4DTkIneAGiVE5D9sWnhuIqHz7d5%2FGkli9QDAiEAkeHMZlPk8nbL6PQ1PtidQS6PbpDqBqp924MB57SB%2FNgq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDFhUP%2FIHgsvHxSx7JircA12TXMPLaxFzLcjPZ2tYL8pDE6UKThZmgHl%2BUFoEpPP%2BYW3Dw2GZQwdk2VnCM9BEOShmDvfA%2FnJx8mURX28uG6XQ%2Faxl9PIGc32%2FuQG%2BXgE7wAWKE5h5b0UtNayW5SZvdRQ551UdYeO4XvR8KTO9nxD1uYJOv03ts6LCPp7%2BN4XTtU%2FFu3yGaWJ7vJE3JCnzWd5yFWRxmWjRqZkEvBih%2FjMnfmW8XsHfRvmQ64osYhW%2Bfhis%2FK8JJVkDRevZI%2BLKBkqigt5%2FKIL0Fat8ABIlu2qIrkITVOXeh23nIgSk3Dj%2FyGMQjBEfDKTvy9uhAifmvXrL%2FMTpLIDSoa7prKj%2BjpQq0qWjyJF0jiJTnTeLYU0u5cGC0VRgaAb26zjup1yg38AxJWTjIza%2BHkARMmfULmJkXPLnkH%2FrkCCE1FEvb8vx7erZ4ZKVC2ZmU3TFh2cJzp1YLTH8w0kE104sBJ%2F%2Fmy1oKgJJiwPM%2F%2Bp%2Fn5YimvV%2FCG4cVnqBxtb%2Bv%2F2KDnI0nOyMSzj3LfUDp7QCxWzC1YM%2Bj%2FHBvscfT9Rw8qLOoLgv4zEHuRa9UqqGvS%2B51ClMN74jNiToc8Soozl3o0LSBKaxicrDd8TUmYl%2FaC9%2Bsu87JNVCtFfdUsrOqtPzMMyX9ccGOqUBgsoMgZ1Ts%2FJ7KiCheArkvjwhsA6bs%2B1rY%2FHTqTjmir8UWOmUxzZqHyIX2a0qmRnGQzDAXPIuwDzIRTDfRpCTEdBHnLJGgqYAso%2BXcYhaoFHBtgixvYb9%2BdDyN7J4lhaurxNeY3emQaeHzvGYKIicTu9%2BjKFXQKXYUxSKRLk5YTRYpoS0a0A2b9OpaCLlOiwgjvwrzyfpqXgFivag1jERHqEB8LIZ&X-Amz-Signature=6008ee3f1d9db3557e17f7f925e25a248059478602d972e1266959c469762d9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

