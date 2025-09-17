---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZH7EXCCD%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQDE281A611EWtTkfJKPLKk4GYznwoSw9811mXdLckd9RQIgTP5G9pqUf1ujSNGun5kjAuLYIFRwdqKTAcMEwxcpYu0qiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBjvfAhfD%2B3udsJwnSrcA36HIO8g6gc6lmSkH7zcIgKMtHFR1w5TsfFsk80sYxn1hZ%2F4R4Qzns62%2F3ZmHZBVHiOSbZEmDX4xfYgxYBZOyo3AxRpf8TnVs4R3FdzqD982hVLG2Bkup2eHzq8zCidKM9yKuk2hHwZkf9OqG6Jn5%2BkKlz4QiFo8sCFQXpzT952%2FTImblH0E7M7wToaVJlhbsIvzIBClbhg9zW1BnvLSWt8CZ8hg%2BHSFeVCy2DTs40H41nRO5GSfgQ%2BAZfV6kwvq2ESij3fasscrXL1JWFsSeAFrlkcAmz4EN%2F4kmjfNxIbB2W%2FCu%2FEuisfQ%2FFNgcerVMBEGMeLN3StGehgi%2FJFsBQmbBqm4PuRDRqq5L9Qx2571LnNCRqkC7NYxlYz2Hb0Wg56tZJLE99TsSdrWYtqfOm05gV8pelOEP8QXkH7p%2FV5mYf1F6pxsbzG6NPRgsyF82HgkZkWgSPwGMJv06KrdQbVdH9sAL%2BYzKNQyfufbJ1e3liAeV%2BnKQvMrgSoDszDKCZ1f3uDCYZB7F8NStyzO48UqfiC%2BgBM4j3%2FqrbN6vS9%2BQZSJ%2FD%2FdjVw6jIVbqcCXy6ZbRVPsAlnargjjtANsDQtEiGSQLxU5ZcOoP%2Fa9R%2FvyJjdgi0KLXkcqxVX2MJTFqcYGOqUB0KXZUowW8LHfgKqKuqhwPh803MLOCZ0VP5qs8igb69jzBVv9%2ByiGrTKsva7NZJ0IiILm3r7C22bY%2B1INvuFFvFIdNBGBHwZp2yIyiB4rw%2BmsBqpMJVJHiTWuphpDrLOPKTtlyLL6szMcu97yjbn5w5Ww3nmT55SYrhXPKQ2Sk8nSKVaynWZSopisrP1AAhc%2B38cJRvYBKHzOElPGjXBS4K9QfeVS&X-Amz-Signature=d2df23cc708d8605ae85284688fc38a6f52ce9cea38a767b7701e7d7398f433a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

