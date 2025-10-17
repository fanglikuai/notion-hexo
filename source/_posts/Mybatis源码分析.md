---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PNG6Z3F%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDfHpqbKyCJ%2F9CD1MJZ7SDM1o%2BdP3O%2FZj097Z9x%2BLPzGAiEA2W9UuPB7XxovLrNtF8PEqZBYwqm146q4sB86IrFNAQIqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOpwCNnqTjxEsisDfCrcA8F74QnDCLQ0ZTqU%2F%2F21p4ICkpFWORXDihJ0ja9SQh9BKInL%2F1PyvcyQxOszzfHbMCQofBXI3DrAaGfJ%2FkqI1q7m2eiYakPtYJPKXunc57P1hV4aohNGRq8CMQWlY3kt2%2Fd0ckSlpF%2Fk%2FefEaul98vJQp55YBuRNMhVwYs7aVUs0nX3BXfiCiq53X50uqGqykWtwKj1ekhefXmQ5%2FyKvYtuePW%2B5CwLxPm3eZxmgDkahFT0Ks676ABp5fMRYaasmLoypcPzZvMIDpYcdW6QYbGPS2F9l7HXeMmlJTPsQjKN%2F7sDy9eR3C3R2QmVOdewfKtLNi9nBRwyMLFwu0uZ2lxOE0Bt0lytABvunNVZAaCiBlbAkLbYBD2S%2B0Owz%2B9ITowVQXGAUaI5rDa4ZQ8hqAf31ZKvGjEPZjx%2BuZ4HCxV9bsurp2whXs%2BYRN%2BZGahOpOxeFYp5ult7PqdZ2bBe0jdoSehr0PbwuyW8hDvGI6dyGIDE1isWVkR8utiFPBOoitysGNIWkZP0nLvBw8xjw6pSUt2P%2B8nAkAnHp7S3tUo20ZbpQ3Wjdnv9eZSFu5aBLCrwRXh7fl0qAbjIIulxgxv%2FnFVBFJBJPG04ksCGPr9TPpDp9gysTAWdM9qzDMMbBxscGOqUBGUAlKTewJ%2BivNwUdTPLU2LI6ZmgCPadCGyz6PSKfLLOPyTTSYti9pT8l2eUSDsVJOXC5Ziv7BWs3CmIJkE2cQtAntaqVadQicLTxcfyoXAD70%2FRCBmAuAy3ySTJANsh2Y5eKV9kxZZZpjdE2ekFqgLWtZoF2IOIEaHE4%2FCBJpl7IV5BA8Er9dHnQ7Ioj%2B%2F7h3ixXKuI8i2jqgSdE%2FMHXxbJ6UMS%2B&X-Amz-Signature=bb311740ff0d1ae73f84c5d7fafcfca9a17047594d160214f2ea6008da620541&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

