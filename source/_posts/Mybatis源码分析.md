---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBF34QIM%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDuqQkNgyBoMA%2BxnZ2SVXwgwvsijwb31f8nfxN5JuVsgAIhAJBpERc0WCuOfcrnf%2Ffw7nq9nlFdXGEUUdAiUvksUnm6Kv8DCCgQABoMNjM3NDIzMTgzODA1IgxL3K2uWdhrX1jbTL4q3ANY33oLJvlBYgb3JCCxaRkyMFRBR4vTe1uooWK8HRAC2F%2FDJoOKXwJaOHLrLdwJ1d%2BZCuJeGBX%2FYpZxZfGZ9L9X1SzKem6ymoQPF4ND1ECU%2BMNEnaZRzxqfmPKAWXH13DtcLevCrlOBGi6tyC48QS1RhT%2BcRL99Uj9tnVXgIVKZu8tC%2Bw082ZnH9a66DtvPlutdQgCRxpcmd0vhlcEtsqEiNwRuzkLWLKoMwVLbWA3Dtx6n4Xu2FdS6ZAKI9jnBmIDcuSfWwRcPqJ9OPWSySLyDCJ23lUSjI9luxgDDZsyiyQwHgQKF5bCKC95zTNSg3Og%2BKnT6vGjM5PV%2FzCZ0IwaLFL%2BfUGhwe1Irnk4phIGx8Ty%2BVZgHs8%2BkEtDI9bKNwCfxtLDazqnf8HHScdPSIBUSMdY6hyk8OLLSHJQ4qkURfWM4gxTpQ4z9hGc58uWLLuFErUWNLkDWhQ53fgASYYI1nbuXxUwfPySoXZHPmCDqYzTYnfu4xazu7pcWGc%2FgqqiZ%2F9TL2jSZJywhQ6k7r%2FQcoHs%2Fwnbe%2FoxxPtYX6y%2BLuZDPGzetlc9TLjhrTButnZkXu0t5gsIC40G3AKY1kz0%2BSytCGq%2F4VN9W5JNHvgP5%2FfMVmRVPWudLUaFJKDCh0PjGBjqkAdArS12jJjOogULvu787x1apgamSNnryDJ0PSYpef0BKWqlEJjERtI2L5s%2Br9O753dXDeK10hw5hBGMP75ecAvYeKri%2FE9vB%2BoRjuC0Ks1SEqgPBcterzt0j114QezP35XVpNxbHTWKRyOKhc5zWbCYwluUAYCkCJi9cCAVdsjIqra5KYqQqBDGYPPtx7JQWUtT4QoXZWuCdQ8PfRmZjhj7c%2Blqf&X-Amz-Signature=62cd08b02c7265df1a6717be6ff112417c91a9e3dd876e97876aaf06e793109e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

