---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHZEUXMZ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5HiomxDeb1fMr6eKGyQQrZIsfm64zVy59rayqGt%2BolQIhANPOjWLMCxooSLwfOcfthQO5lHN7rO0Rq88spQdQGapQKv8DCGQQABoMNjM3NDIzMTgzODA1IgzaLX6YXYHmvpNr5gwq3APFsCBUvA1ln2usHbmSYQktzDqqQbsBAlb0jl%2FF1mCgVG9uJQ2VZVkvwjRsoy%2FKiCkuVchXBhXwIOHV%2FDKqgdKlQVbPN02EQRVf2DCclKeQ9nkI5i6fgh5JoKBHYx5OQkNPtrmSTNm%2FAtBAX9fDbMKx8l6D6s%2F7bYwR1KdQAP4mK2M6viuWn3xMDHqfiFaIiCJdE8tkXFOAD8lT5RxiFe7K6%2BpNtESt2RsCdEW6YdhdTWUK6HnoL19lxGpKiWPI8nktVKO7qQqOy1XK1EAMdmIj3qwwXQFnhJT8wJ4civVKDfWfJx%2Ft3UPqfX4JmQtwymgX0uuqFEGm4NqYQLD6KFxKXYMdxK7%2F3t2zewTfJmJ3S3rvqpY7SdbAOsJoLvuDwnqQMFmAroAif6nK3baGwYB7FNSRWErKsCUnbxt2UGsCBJWG82U1JP6pbBBTEdt2wDUrPHsJNj%2F5eNaf33TdJasUkeg6MhM%2B9OE8eKhVgxs1KydQXg5bLFco2uk5mjT6sE6tjddfKM9aQyUmPo6zXwvI3cb9f8aS8cjIKnqxTzhbeAm60oLX8E2jugaAhpUzSXXg2TvJLcBew2lzMJtHPct66cFFwAkUAS02BDZ6A5GN4tGJw7VyENflkSRvCjCq86PIBjqkAVAeoAuDLVycwWp0ZzX9Vulv7lpLeBEQEshT5Lxs39dm6SuJNaPIH3d55HV2dfuwx%2BlZ0KbgTEcKYEybpQvTXeeLXQrAKPPSVoO7WhrR4JILyGkVpywZNfTibhTdWgC%2F5UjYsg%2FhSpRkjFxfqZgOn3V8LgrW4L3k6stlZ6c7%2Bi9MnaxwB0HnsSypjLcUaAv96F4ft3qTqlVXbC1389QNZ7D1FODO&X-Amz-Signature=ac95c597c1492e5837b8ceb5f5b65e89cce59c7560ac2130fef81c9709064f54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

