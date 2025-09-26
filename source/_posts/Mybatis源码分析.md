---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVJ7DYS%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQDfg8VLiDvFh5IzCtI61Lv81oiWIEILD%2Bj8Fm%2BBZevhMgIhAKzckQ2KeIYkhuxbCI3ztUKgR90vCzF2odRfS%2BrbfFnNKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBDcOtTvJZp0Sri%2FEq3AMJgiU6dOt0c%2FT6rHb7aB4yqK44aGGzLiM0oHjrRmKkxs3WPDHO4B5chqXrM9wdUtySxaEbxVRWTaNiQNsenlhoRT%2FUmVYT8jXjDTU8YuFiXlRNXFJbFU821ZCy4%2BCRFIxJSgistCq%2BP0EZd0NTVp2OgwnZQMbULK8i8FqOmTCrwtlH8%2Bp4woa0W6yU2TnmQP8FYw%2FgGEYgajrbcgwrI%2FDy%2Blq2JncSO1FOanXrAzQKJ1ZZe5njU3Dr063yEW%2BMYaRTRwix3o0kgyZRDarSqyWdrdU6seA%2BnBjuP%2BeKfYo%2BFwAqxdTwFIvSAWpEW%2BqIR2Mvr1tpMC%2F8aHlxhsBeGX2mut1lET4CvZvqQOlLDi67iYlajleLWqUso9wN7rnixZuAIWdK24MuP2YN2JogdwHmnDMsV5jEePRiwkapSuSf%2BcizHs80UZjGN6DHdpvywD8m90%2B0u5a4Qp8npVtcirJ1IHhdgvo1kom%2FviKDPuAQg7HRH%2BxqkFJQh55MW7txvhB3eUKv0Br7gt069D0unm3sXY5v0ZED6XBOIr%2BFLdXQOlGVZsvKsrVW6EuRRLcR8%2FhhnPg4lxN4Au8xnpKid%2FYHHFBE2TP59BfQwOQLImhxKpZ9ZurtuEE%2BDX1H%2BDCy2tvGBjqkAbhXefOUCmUzQXNFKeB9I4AsZpFyh4t9EAhl0h9U5jT69%2F6IW437IIho3bSK%2BZycpFwIPhyozp5RIznI%2BbI7nLylzggogamhBpVT4f%2BNfhT8XgzGWFuyfBbAVz0R1pFaOy35FXC4hCSvuECb58DvidOdpDHZz4yYozwyHE40xZKXi5eGMFxsXABmBPq%2BaMXtKeQcWQhHIoP5toBbQzVyRpBcKSDk&X-Amz-Signature=9f585dfabdab420d409e2c6719fb54bc8e7800ac28dbbd66d2804bbc98f79ba9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

