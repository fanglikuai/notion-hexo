---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI2PWPH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCICw3OpBZsXPhmUsyLsXODr2F9bNxTIMVwvxLpYHbpT5sAiBvP23lcOsFvQ2TV3A%2BiR6UnsDDOR4RfjQYxz%2F%2BxofKqSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7jWrb8yVL2C4z%2BL5KtwDPgN4Y2PM1xCRJVNbN8t0BvoHSCxVmT%2FEQUWf%2FDPRPI1xSzez2rhf59%2Bfhs3sUIb2o58KWUhfOJuxVmfk06sn5qQ56z9UhneApdpVlaqawIbLW1skOmNqo99TrhyYeUCnkKHAqYBDfHfxpiqg5jVhSzKi5PCVDTYyFYlAX6TKvdyGgMUGCtVe2krFmr9661TPjY8Q%2BhuBV0aFqz5YwdrjYq0Vl1hsPtaPF7TwHtPUM%2BFxenN6h5fRVRJYh3L%2FSLMqiejTIoHLpHEmGy7radmlaeEqkmuTLKZ42%2FOaOmlO3RwOyO8vC6pGgdf85cnMwuOS3m4pvy%2F1LRfKDpkGOnEsCAoJHNDnZfZ4lWzIcyXHMJlRVyUMVPi6mdXPUTQki%2BStDPiHDwV%2BhlQcGKhWQWnL7M8Xl390WT6ZKwkOyHlBlpZ2%2B%2Bf133b%2BDgd3bQrl8YhH2p%2Bf5bGEY5KXyeFo1Dk61TvF1ycgTsIaRcdKk2H1WHpuKTN0ZQshdKkz7Lr1rSFYiTTD29H4bYJ4lT5O0P77tjWLsO3I3nzac11kH9XvUBFn5YMd9sgHE6z%2FQ%2F2jz3yTYaYPZviM2Qn%2FBAqb5e%2B%2FHWo3JOtRTmOgmgK%2BGVcbrFM%2BuTrUoTtP%2Fg7iSmgwu6SgxwY6pgFtolR3LqGCwm4FdVwkll6XpqzyDDeq9ppuLZuKb4TQo%2Fi5Efr4ngaGBdtzqdECRLg8ItdgOutf4ofdecDJwSe2o2ZMHj4Uw8x4Db53rsu5zuD97ReOKNS6eDpde7CyHTzNllxjrdRq%2FlD%2B%2BJwL4IAz2eRjcwGV%2Fl0QV5NgBqVNX76m2Ay46mnUIprXqjDblNJod6tAH5ZQe5DgvqaRj7%2BLE9t47tAd&X-Amz-Signature=bbce7348259a6446f822f03b8f5b6ff4b28890ccce22e509a59f4926d0671ae5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

