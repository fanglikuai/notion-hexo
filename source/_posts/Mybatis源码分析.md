---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VY25SN43%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBVHtzif%2BhUmdWjqTCav%2B3538vHp%2FY5GXQ2ah3QheubFAiBczjIasbc3%2Bn798%2F8VDOJrwcOohvIJC4kLQCRLBM95Kir%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIM52s2MY4W7QJqgyQxKtwDmDxB2cNd5fTH7jtQpZJTZtHzr2aQgiArdisJ2%2BERJpNhJDJZa5205PCCAawzqUnvBNT4NRj3VDdstuOo199ANtBiuMSOYWde5W%2BWkw5g%2FOv%2BKqGxAF%2B7ku7IAbzzcLYrEut6TUaIaLoyBzIP4D5d8j8laAD2Kgp23qWNjVxNGj2ufmfr5Xy6pasNx6lWgiq2Hnh6tZ%2BJktmYsfQDkvhS7wzYYYu%2B2r7qyDOYjzvSpraM1ESDL8tN%2BG61iA8j8a1gBU3iF1R%2FOnaqyBVprrt9XOJs74JA53HnYCm3WynmAX8YhjQL2vefsNXpG2xlSfVHy7Fp%2BR%2BfTbvfgm4MMQQfjYho4ZMoJK%2FvtT%2Buh8en1rTQxztXL8wh86lBDUN0t7j0ERpIInmANPsUMvf4fRfxH%2BvrIAD3TWCoNMv5AuOfcMkW5aUHZ8d%2FiM8e5v1bvJl63XpjHXe6DIG9gw%2BmKYEiZj5Z8HzYpTaUYiz3DxzklzU%2B0q7HPglnn6KKJ6WlqsY3xSAiTldF9sui2u6LnBv2w2hx42OK6N%2FX0gQd5MxJCkM6%2F7ZfsvQZJvBK6d9ifmnONA621f%2BEOh641uAHx81CLpiijD7V6yzhC%2F8N23YNvxKLTdsgwL8it9EZ91Iw5Im8xwY6pgFtaO2LNl0aBtle7XKBirG%2Fo6N5hM9P40zjiQalVCzncZCalYRLBEGOFKdEBaoGgp3PnqUEnOsxBtBcOYtH1ls2ky7Qnn%2BB%2F94KVxq6HcdS8fpknf%2BXcpf9huJjr3GohUmnZzC73VpUDVdg2Q5IxPXeEOWR%2BycGxsX6993oN02yFBqW0mUfLCotF6S3N5KZyRQ9Eef0fCSJ9fTL888mxWnSbmi4b8Xk&X-Amz-Signature=48664b7393424dcae8b306b768967e7f4923e39394886222ef15faa22b150d07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

