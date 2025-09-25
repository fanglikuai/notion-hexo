---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X6XNV5K%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrWv6StsEKVow%2Fm2Xg9FaWxdGMO3uGHfnsHNnxqlB4tgIhALkmgw24ytIqn%2Fb1F0BqLsjAcUJHQ%2FWIAF2otJ38RppVKv8DCHQQABoMNjM3NDIzMTgzODA1Igzhu7xhwCklIC6rZqEq3AMyBOzuVzOFwgdTdYPXzQLgRY6%2BNpgmC7LPwuvp5R1fGA%2BX2DRkNh8BNAMMYOvZa33BsHA1rsnPeGZe7wEBzkrCwlTCW6r0Z6GyvVrl1IfxnR24Sb%2BuJY3Lv2MydflM6rPo2%2FQBu7EgKZAXVwaFeBMlofDRe6zr0x41IFyBoxztu4mkptx0td7ZdusvdqDmdtpIcfcUzl60unoAh2nkWGECWsXE7MqN9DrfhRKgRrtYbPcM%2B2w9RpUq1Epq1Y%2FmrY23vxyP9gGLjho8Ol%2F0kZxNWq%2B5t6sPJ06zANA3CvFC0QP80A%2FVMZ6Hy0a2nB5gDrbLBVyVZEJuZFbQQ%2F4evyHiNTjd1aTxs9pHYPLXdLdGKTqQh4u0f18I2qL%2B6oYV%2Fdbp8T8p%2FtriOg2luxGD6wIjW4ZDKXndt8P8DSURbqCyT%2BSfgPROz2Scbbxi2Ff1UZjhfnQsAqnmN5XH27swLaZtw%2BQqRdyOMZomV6rrgl%2B07vPQv%2BB%2FbRe4NiPKfOuxTEpLx6x4qP%2Bxg42Doit%2BhHLJZLutliEk3hehTttuS6UHsVrxU%2BmV5eRz4aXk0ikFVvkSMJA4%2FMQiBRsgGzWN%2Fg0Xwrgy8jsjikto3SUGs2hkRKVaPZpFiRPr6Wd7iTC4vdTGBjqkAcb4uOqQ%2BsGiS6YtMXm8MD%2B%2B2LR05NgZuUoxU1zJpiz7Qbq%2FjzRSjAu%2FqaQfwWhtVEj1K3O5PD%2FZ%2BaY8LkYmbPZN5t6k6gLMRcsjAndFukegNKpbfG1%2FzNC9Z4hRezCUziincjB3ZcvaEqzBZXuaW6q8baJC799YfWuQfefYs2exLfziEgt3FxUhTxAeS2d1iLWBho4sVNmb2JgbaviwV2sECw2R&X-Amz-Signature=6797e2f2c5e6099f1753c3b795ca5347f1ede9ac0139338d8164251b06133224&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

