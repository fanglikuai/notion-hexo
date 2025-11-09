---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QC3RW6X%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIB5f90S9S%2BvDq3WeOj1ZDIQTD02SbWrS7tyfqBXdUj2FAiBENPp8kKc6EVJioAQPanORQQOeV1njrs072uItze4EkiqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZwDpRU1n3UvtE%2BWxKtwDKBWnFIokcZZ63P9jMBP5Nj5ufW0%2FFTD5ckTwT%2BrDWqo4K0L%2FIflVIMxQNm42YsDIK1IKVbX3275%2FhnRxGmF9o3pK2mxEbDUNtmD%2Bz1FY8zdjpxFQ3gPJMD6oiLbm8MhM7ZduZxnctkGJs5qclNHfQVhtiBkPguzy6e%2FXmvgY8fANcnkKDR080QlswHCGNZ6EMxTxsF873gXvUzS4PqQNyzoxDV8gUFDmbKqScczWwCMDhjFShFwWJUJ4gnsUTVpY0xTq6nRfbq4x7UFujJnb9N20Bkf4F7U0Q9IjQkQk0iKEtfMOwhMgziRh0CP%2F2%2FBBa%2BPV7SCGMEQHa0rYaolW67ArvvrBCq5Loa0YdkpVdKtV2oYKV12C8Cm57NpTYfE%2FAcO9cOYNu26zG91%2Fnit7qeWDQClKxxOEvBN5%2FHuhP9fLWsgKZoHQ1j3IY3Hs7P13IDLH%2FLkfMyOGQhni4fWbLa%2B5qTUvKEngBLmxQPnMpcp%2FJk3u5%2B7gBA40od49Pql7M4zmlo5Jxk22c198DZCdUdMM5YSWQZxY7aZ3pOo33v%2B19RFuq0u8b15ehHmncU3hE5T6AjYkoBDi1eKAZQEpQWwhZCAuJCMXvSjHYFHliwdLSLNjOaxQsV9Tyi0w9ZHByAY6pgHTMzqmuYjOLZpsIRa9we5M9%2FSBemMMotD69qnbU0zwpuWcRqbU4v0nmR1cN%2BJIl4bH5kPZDF8C66c9liVjDHpZnTfefKAx8P3aP8LHvGQ%2Fzn8qsUplUwqJj5%2F9YyVmEfAd68e1mbP740K2fl6QW6AMy2rnLfR4pi5VPrDetdOYtwNM4OrQh5%2BNR%2BCFeGQmLA9mAzzIWSrTQ4o1CO0udt8SNwg4RnVS&X-Amz-Signature=6f0e48472a6e45d0d0ac5382b735bac011e18cb520b2fb9715750ec6d3465b7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

