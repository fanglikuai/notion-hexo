---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2Q3E57D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWn2mcYNNYeF8HPJBvgpcCgcMBGTz4BFo6Qha8uSJrLAIgAYCJBMz03vDjkPN8H9rR6foXOU%2BbgyvPibYPO5z3j%2Fcq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMTti5Q6IoZNnxTy9CrcA%2Fxk2PsSqqYkgk1gilV71JSiLAAoeN2i9UcJk7XT81z5ZjbsOAXfWEwYkK2ZLTyM%2F14irw10IzOVQIJSPG8g45zxfkewDQ2wHzKkWIIRnXVWZWqldL%2FduisOj8zOCrBsr4Q8d6erCubYacUJnFE77ujMgjyhwRD1lKYLjhrDOnpiU8rYnf0i4NZTMI0v37L1V9JLYVi6oYf%2F7PE%2B%2Fc5L4ODsyThs%2Fyw%2BzXZun8mq7htkauNAZKGPJTwDBP2l7wGfrO1y6b7SAk%2F5toONUbh031%2F9cCwtCmM7kAqLSMqmkRhF6CLVhHN%2BboZZn5%2FHykTzTFmwgMSTZbupF%2F39YKBlvYW3NSU36hWtHFdMFrpXNJvAHMMz4CQVjDtWnhA4u%2Bkgr2S%2Fsn3ryi4nDXmTGk22GmWT1FOQg9tkWnneaYIflwUEk7m0MUYgs3DPPmZ4mRlj0pYUr%2Fe0M6gI5rTKyf50dhXHdE1%2B%2F%2FM%2FfO5LKXvnLJLXZgedZAYeKe7chfjBvwvMaQ8etstFKlXCE0jF7kKL2J4eyskKOUBFtPqR1xS7MbbUig%2B7MXdCKQP1sGFSg7xf5PImEMZGKJ5et%2F4GKhlyXAToLo57ocyA6DNhatD7x5Fn%2BsZ7QPO07FXF74SyMOyvp8gGOqUBGAQ09aZoRVgja%2FAeP4YdmMRJmlwysm1ntCyXATrAqxzzlvFEkzqu%2FleqCyOJI7M4Ur2Mwka9yXo9Gy2WZiE4hRROc9jTIXKiRU35T2TqALCtQJNO4%2Bhg1qQr9nwE5uk6DCiVIeMVlN%2FSLBrwrupJhMGADYpGP4bSoXKxrBXq0sHfdo3DlLkQoh%2FhHMiDHengr%2F1dKAppEgzWmxdvYV%2BY9MGjHEA4&X-Amz-Signature=4681c4fe919a2bb5fff2d919cb077d7212f7ae16c1279119e71356d37820d3d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

