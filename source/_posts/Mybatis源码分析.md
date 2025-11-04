---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBMEVOY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKmZwEBHTBaiZO%2Fs3tGmcP4IfNqJY1OQh5mn%2Bl1oHweAiA50QDpHPknDvVovbu1DAUi9MjKMHEfmM3AJ7xc4yuB%2BCr%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMNRKgj3poxFXH%2BDHJKtwDGq1AnMtLNH%2BrYhun2%2Fq%2BAX2UATbSFp%2Bq1hUY%2B0kP3miZs50XIRkhnoAO%2FU2VnO1YPYL%2FgkV9cskU4JFjIwL73tCXTltxQQFP1oKiTzAdPAVVr%2BsCZs6k5s%2FJyKsX6tvwZPA4KSZXJBQZY1CoGZ8ZR7neaW8hYj7TrV96d4AkQRUcFSE36y5MJlS15SxOTbS0xLP0JjQuoGm4n0Vu%2Fg7gNHkRXjAbmXNJWoO0NT5JO52u6h1b7EQ9NbSPj0gFOV%2F2ZmP9BnXijr48HWegHzlHD1uW38j9mqQWNMGYoRnI9kw3Ja7746qfVzUqS%2B07EJqeF%2F4AteWrrqMQ2%2BHNTsH%2FdMDO6OQMQp2r4AXtcs9nRa0xL7HTDhWyosNxseZvSG807DWOMjF2zbeti6Nreizc1CC3emSpk6uJCDnOHkDJ5w5J1nljAuQqqoz89oZSVT1IrBiNchP7eYKrPpmA0z3%2B0GB%2BP75Vt2khGbzLKRnYoS0ZEAnmJV1FxasLtdnBoWMGnXgyD9SIAMNKhtfjwlIednKzpM5csyljKwhMfi6RnPWmxcRnU1WuZZ8%2BYTNnt%2BaB75Kz7HGjf5JcJXpsCAg6QKDJk3SFunjle1bPtwpxxEhGm75%2F6V33LH3nXOAwwJWoyAY6pgHtOE6s5XbI1p%2BcphQfFH4lh855oszNlEAxN9lnL5KtpPKfC0VNDLho6pe97s0FQQ8DWu%2FklHK8k2SLF5%2BguJzA1XWv94rz4To4vjqGQySdc3bK%2B1arZcD%2FFBVj8LiqkmNs%2FGebNfJQrxSI%2F6i9MnqJwB4mHDDzWcxWv9pCFVYjxuAmQPyTXWcyd68Sk8qi%2FL9Jyb5gIoDM9eGE8zJKdE4glJodfV0O&X-Amz-Signature=19e4b33852276d1704ebc81af1fcf1db7a3f96dcf840f55d57bd582c8a306ec4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

