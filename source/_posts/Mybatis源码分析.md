---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T24IOI3G%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqWRH9U1%2FUGObmpeu1lA%2FusuQcWVZcm2PymXYjCUTvGgIhANCt4qXaFQRF9HOPb9ELwZQyuBPu6EKrQBDIZtvOdjGeKv8DCG4QABoMNjM3NDIzMTgzODA1Igy3BBPzNtxLY53C7MEq3ANQpVbEzRI2JKeT6803RewN9LxTn8eKZGO9iwy3xBUrqpVqlaLR0K%2FGh6EtuNStUjgAidLZ%2BdGPzfUGF2ClDth%2Ff9mMOXBcQH3XeTqPjzY3zIqBGH%2B2aYstUVrCd2DQTzqBmUKO7U8WYzcJKwhIb9YvxwI2mI4NW9HOplskzbGBqGrKB%2BhZMp1J8RQP%2BPSmql1XCMPEjgTIvAFM9MOXwkGhpNVDu7orWXBEkBLuQ6GnjVPkTUO8KUbAJZljBLUzq8090j%2FEORxPb5hS6VJtDbubnhpy0IsEQ9KcvmHRYP%2BH%2F9HuFbOD2AQ2Em5Kn1xlRmVrQn1K8%2Bc%2FxdFTaF1b%2B081Av0cytEmAW2%2BTAQZ9PWIA9yjTYfwI6zBEFMHUejdwpHugcK0eCwjvJzhGmRAvTXYqKVvD3bwa5Jy4c7Af37cRjGi5tsLpsTDYOXbTiYXpcDcEGY9iTPS1DYArPjPVd8V%2BmHMqSZ4o%2BdVSYEOBbyXwXFUiF3v5fFbXOn1Y7hknXrHg1U%2BLIIQUuuPzm4c0e4ZR0VjDo4ds8sNbjMZeCbAPRVvPE7n5vnoiWT8DP6AUOEuXX6xfotybe00ZMlxO7SAi8DYTx645LZwyRPECTcQWvSPN2BaZdUODVEhxzDOtd7IBjqkAWxPVzQJeYgavJ1qPytbAwXFCw8k3FYi0uSN8ij0%2FesiWy4LFhuQW4CoDUCOPMJSyQM2tyVGKFr0IAy9EJIHYGRihSjoAuH18XUau5t1Ha2V130BibpMcgvsB1XSqfRcflqtHc1tY7b7HQBsCLthknCRePt9W8GT9JBhLdyyXwiaETuYSlAOMJ8L7hbo6pk5pMb37H68xKqNCf2LpjVcoP41XXuY&X-Amz-Signature=840955a6fe7f6d15a68d0f6a2eef855e3dc6a6211c87d665af7fa95001f3cd8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

