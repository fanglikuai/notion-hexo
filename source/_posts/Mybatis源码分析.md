---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZPYGU2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPDrF6iaJfWEY8YhPQ1%2FnHUNZp9eW5gGjRfrXG8e5qCwIhAM9AYyyP7NdfhD3eXgO5rJD%2FOTzJbK6CL%2B%2BqdJI9z5ixKv8DCC0QABoMNjM3NDIzMTgzODA1IgwCvuWMr77dBdSsgmcq3AO21vs87dfYmpWaakLSeaY67u0j73ihpxqVvrq7QU4Dd3FbH2txVVHI6ZK8uxWrqMAVpBHPUEw6mx7itfCZwbtPmE%2Bqjsl%2BF18trhIGuf%2FbchAysxeu6oN5Csc3NnVJkpuTpjeDgGQCvk9ty51tBPfg0BRubabJJyKqY%2BOoQRLGGfXBll3G6hkuqOQnl%2FA2yNZF7Mc37Q6xCdcvbjr612YP9%2B9FI6qJfGm0CUw5DqrNwuSzoLyVRp7a4Yw5jt5Vk6rdvQPjlnchqIrynbv0N1NHpMUbk5iQFhmacY4OexDolchAGtylgkjc%2FUdDOwEKUnho%2BQ9N9xAxuQjcr9KlrMk4WLrqQ%2Bc2xHJHv%2BKFwQPwY%2BkNNO2hxTbX1jlPcJzw%2BWe9I1renwCBbO%2BJMOTxWLJ1egmeZMp4JiFGw6Tq6IubywCOeGCsw70Jvq1pU0F8fGRrPf%2F%2Fo9Szkly2Gp84gUzYXKwOoc8EV9qJ%2FyD1z89Gr3Z5mQHt5DWuhKNXfvvgmBEXCbtGHZfL99nP0KHs04fYPJDCPTUG1Y0FUZvtfBnXEs8xjT3bZAu1Q6wOP7HZtfGXN5NfGB4VS5PS9%2BkaMPYPYaUD%2Ft8LMcHnjcMgpGK9xdiCCCfA%2Buw6fI%2FZojC1w%2FnGBjqkAdiyzjDnZmwsUopvRvAobR1kK5kKLnMugbgHCBf30M6RGSOefa%2BIE4QHYYysjiRuzlv1qUw0oF7oEGOWGGNMpFTD%2FNP3coLSJ31b%2FXPy01b5A5gzAuIBWwLTN2%2FiZolOxbR6R1a054twaPUPqaEv8fHVjxPrZoXuO3ur7iFkeTs5VPKYilO3Yx%2B3%2BB%2Bd8WTaQqLCi1mrNDflUhcCranWwbpHM96U&X-Amz-Signature=aac7d7c19992b772f6b558f240ace0db6c6b72688c9c3fe6f39d5fa8ee8c9552&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

