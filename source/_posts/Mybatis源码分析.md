---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W4LO2AQU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC11qxrXiH656JwrCnMfxsP2%2F6zldpsOT7gDerruejScwIgNNWrWYFhG8pLEpFNiDZfTZ%2B%2BAqKqm8GsFMngCXDZWgAq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMMAxqhLX4qqvzxo9CrcA5vzmRvPYkh9rsS5MGNKOrKLbW6c%2BKD33tjDETzNymbR6xKMKj1w%2FVnmnxokL1jxaKsb4zEEsP98XvufVTZjbPDfR%2B%2BxQ%2FlPqbh5xfbXxhDcxiE9sWSnVOdLINNolWRqrjeGC191uiczyHJKvCwLa4TSPg9yFAa3sOTd7VXP1s%2FmAURLd9cwBZFwAtLD5nAE0H7ALpKTMMAkS1f9iE7agNL42QrxDrvlGTBr9UrfwpTKbeSk15MUc82aHsZ6x8F%2Fq1Zr2%2FBdMIpdRwhBguqDDcAu91fTWHOT9HykGnoQ43TaKkgt6xDBVJjVKS7%2BtetIMdyPnhCb%2F0gohRnJyryG95JPXZam%2BeeosWLqSG8MfcaFd27jvFpiDvWI0ACjJwkOnYuiJDKwaWCi5BotfxDLX0R2b5IbUDunMGuQuB4Rsx62Y%2BCi0qhOyU9yU2Li2RCLKEKqOPvhE%2FfDWR0o4Sf90pCiXcVwDgHCmmUEafN0QOEbyJ%2BTQm560sO%2FuLrOiiCoL6%2Bq9WkLPQMxztbo4J%2FQx0yvRxy2N2%2F%2BLrKblk%2Fxh1U8I5U2DN23vaXVYw1%2FpRcacHGHhG03gY3p27fFlJsSmmoUIBpcx5fxJ%2F8J4VXN3dc63XL3J9GSlbHwnPy6MI%2FapMgGOqUBRdPTUS0MRwVRQgAJgAKjZdAcFFLsHp8ud3Qu2tJSLQcVyo28SukigjeK7bL%2BGalde2ASbH2XVxgZ4K%2FLCt7jZprYQQa7DNeF21cb9GF5OM0nGU71pkM8Ili9o%2F%2Fqz7RPmWC5Xva07XAZu5%2Bze1o0xWEoOUY2NeH8JTYaQhWWSgPVl07XMKBPj%2FYZcVZHv9xI70pOFhY4zogdHe%2BYDdg3lLO5nCtC&X-Amz-Signature=9c8f570afbd814c2ba3cec31e8f3d497f6aa8b41949c3154b3f6dd1779e27edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

