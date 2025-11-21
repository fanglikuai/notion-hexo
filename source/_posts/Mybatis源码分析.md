---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVHR3CCP%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIDwk1s9FGPNnVncnjy2V6UVhi6uhOIuS8yOF7aj0ejCWAiBWBNfwI6SmrzWflgHV%2BW8bs9dAahY6%2FJva9wTKw6eVUyr%2FAwgKEAAaDDYzNzQyMzE4MzgwNSIMDu2H2PdlPN7sNzu3KtwDihe5trfvQhiHFA3G5ZD2kB%2FGY1cqs3MdLNf%2FF7SI9Sip4Fcr%2FwspcdWWMHvXtc5ElT2b6706JAy5lS1GZ7MzFH6EDb%2F3%2BDRStjVh7Vc%2B54hlUBzvcuxIyn%2BbJGPafB7CXlo0bqO07bnUsmfQhH9ns866HqyTRuj6rurYWdTP7o4BS06xaMxKwDTqUOpPePFEzOrJJZr42TbNNDva2%2BGnwgVcUvit6N10%2BzE2FQxTz7G7u48dFUt2MYfotmjTtcIU2XDNoWSTS5ABgmHeflUUbMP4eeBltDE0d%2F4eINz9%2BBJR2HV1ywYkP%2B8HpVGGCXEf%2FdcWPY1mTul4DalfD7TRvPSSrKMqfuFw9WxiTYikiuGlhPW5eq7MY%2BmoaGYUpNnEtrykxHUZA4aZHk6WAmcLhLSdoyS8tAOQtRYYB1rGAnKPZacBkYcaeizcs1hLz0ruuqqJkZcWvj5JW99NXBa1%2BsE%2FmTupcCJVgjnWy%2FsWnzfRmFygEq%2Fw8kcVxmNWULLVjfp7FGQyxqxhgHz7l3PfoaAoL38k87lIsiaukT8rh56xy%2BW42R8NnGx1SSUXd88kYta%2BeAFA5c3FAVNN3VwQHy%2FVpvLlzibavKLS2eE8vpMWb9PqvptuQMBxHkIw4dqAyQY6pgFv8UUnKPN8gsr50VW9lEHSR4ppei04FOuHz5nxc9M6cNTBNsKg9SudlCxJTKiAgL2M5C9aCPVQTA7Jxyjwa1%2BwGl4WxoDk73wpJ3whQ8x9p8SCeETIQvzjkMDmM69gt8EpPSFYWJcB0xySl4GKIwLTFbj6MIAKQ1HbWQGbQsDcT%2BJ70fi19tS8C8dfAesvnhc3qovQutlbIC4cYECBukknVFdUd8h%2B&X-Amz-Signature=e7c893b6ff735adb7e7ca157083199df396153728fcebbbd6c642854ec31b5b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

