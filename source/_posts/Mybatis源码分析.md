---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BNDQB3Y%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCEB6TTLphLVgwdoUkx5z%2FkZ5ChRGZ7%2B11bI3n7rhmLwwIhAK8MX%2FpDW1blkCwWRjb0IkpnBkWhUPrgXDocqqZIutiZKv8DCCsQABoMNjM3NDIzMTgzODA1IgwQdt0zKRC%2BLLqNXGYq3AMHhLMHD3%2BDPsZc7OoNOH%2Fs02RrQdrm3LDenPOSslzhLrdn%2FqsZRpfbJlhHXSdxGe4fb31luMO9OedzzeBdnUTr%2F%2F7%2FXFN5lLMC1IGhxmhXQUcmwjNvoCN32JZy9b6G310k4nDvQKJFRPLFV7qQrVkTdamQJxIEV1%2FNS0ihburJYW75XhUlFH2boqG89YUNOFUSfUIyCN0k8U5SRYKs4L45DraBKyGntZH0mEIUATDT2hVxMS8ckgmFgMQxNeP14JM1vpTKSEU3Oexf5VDuOtjJRcV%2BHmYMBmfYj89oWIa1sHS8IiDQWj7LjE7vi6%2BBteVvjwHZoToSfpc%2BMP31WK1mk6bxMl%2FCunqGcoAHAhgvKBiDbt7yoKKwMFSxmgFKpS%2Faonam22Ho6W9TXebzKVzWelz3XeEy7EmI0UG6d7m1lSAWuTb1Hsm169VQA5SD7n4ldYbfsKGYuc8DACxCKSaXkyq238jsbxSQJJ6dq%2BM2Z%2FyTPUlUZ9zShfe0PFiH8Apr5Lv%2Bw4a6er5jqkNF%2B9Q4R9%2FsGk%2BmJkoreGh2TGExSTwnCIj%2BIIdWBtEyLkxkblctHUuJDQoYkrHcCFXV9kLLxAvBI4LWtrtjQhlZTn50d%2BkG2d8InXqoL%2BdPpjC6zM%2FIBjqkAXfDmq5eUNVV5%2BgCN7Gd6oV5vR1wr1MgXFXamixyC4X80BH8fReh2FonvjR5aYqi5ZkQUgwvs7wHzIZKZoab7p%2BNy0rzl3iq7AtfkBNgj8fqDltwAYTs1vkjYkIAK5ReJWPOFeH%2FLbk6kItfjxMvR5UpADRcLDDmXitJCu4eRoh9ltFs%2BGYjzyxinUBuM5ogRHb4YDFvAM%2Fo3yZGWXeDJAEdRWxT&X-Amz-Signature=54bb9cc1e9e95d27f030e32fc46313e8d14333aa4f64dde9c526e9144954ae51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

