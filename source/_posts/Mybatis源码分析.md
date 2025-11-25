---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QTX5BUL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBMquF2L3bZMOpjKvtndK3O2U5o3foR%2F2VzYHL4VOBHwIhAPt8AIKaKiD8t9oyrP8LXbz6TNt0F%2FM2y%2BKzFZCVOgFCKv8DCGwQABoMNjM3NDIzMTgzODA1IgzMowP2BJkG59oJsKQq3APtUQyE%2BcK6CoFmeRjrJImP12UavmDA4SEsAX5TXIb%2FMulsoqNuql9JTA1W%2BFWKQcRaI7POSBWyt4PXFb8FefPyKLDMuQQKd%2Fe7ce6Ak6nvcgQ8SA7OVDaWGOFgQgv53pkKPROl8H0sO8qh5w4dOelJNX0lgN%2BHRqPm5sKJJsVaYUr0QMmaA02ElKg%2Fv6fVhOmTpUIrNfysPP7CkYHYGsPY5pawQbZCLlZp4ku1issIderLEOk3NmYs1%2BSqE0lhz5YT5CtxcqLphP%2BLyNiFrCHp4%2BbpP3QNqPWHGkBFRf1NbeG6CK63YtmKASV9BvtyOPcgvdqyui4jr5hW%2FzqRQBQXZW1mtC%2Bmryzk4sT1a3cNtmiSr6yAF7%2B9Mq2nPiCjF9LbrX7WgSiXraN7EtMYXqyj3K64hyBxNbWan%2F7icLKxK4%2B87%2BVHki9XDDLGYhr%2B6ZPfUdDp6FSvCBhQFxjyZy97IfXkMH6wDF9Xc0zA9PjBnv3ErFTHgGz%2Bawze7I4B1Vord8aIEpPnX3ZXDZOrdRGNoRKL6L8KuZFThy%2FI9yf2eHiQEvQN9Xqu8tFM6Ko%2FsJb97Gh9eIxgoS4JyF1u8k3nlLnj%2FpZhg8toIFrEoKe2v5hrMJJ6Ar4ZJVaQKjC1o5bJBjqkAckLsC2OCbqZnez0K1ozgaUVH3Mjjtx1nQD1BnYGHF6IB4zfYh9RtCIcR4y4ZAYPlPzBnso4i3025uRx9X7BAwgBJWhJmI9MGm1EgCC6tHTqke58ilMGqXwx7pG5UkCvnCmfpfShI18IvNiOH2vabrxSRWyJ6npsgOZ%2FNV3iK33aXg2uT%2F1TXQ7bUG%2ByvS%2FhEwIQxRY3yBJb%2FkjdGN%2FnjuRAmJom&X-Amz-Signature=7e80cdcb3743d509db23c2ee273b93f34003dae472b96f2e9f05de27d1ac4803&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

