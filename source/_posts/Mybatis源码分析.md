---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDHHP6XR%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQC6r7evkeIUzedqR1Nnoei0XblccfcD19aX6CaBV8cPqAIhAKBz7SmbFEmlj%2BBwlvEwdR6cmWrneC9a8afN3xYidtnRKv8DCD0QABoMNjM3NDIzMTgzODA1Igxj59NF8jyGYwReNDIq3AOrwMJpUAvstZBv6WyXT6glV5o5UozVomO2BLKrlVcMnzZKaZNA%2Fp8PVG15PJoeUY79ryHGPN4M8jabZIAiLw%2FNe0X1a4h7RMsDUpUnC4%2F68OXrWvwi2ue19u52zqiWm7Hi16F4s4f2HeaHWKZjAFRdDQG%2FShxEi%2BT8tkvn2jpFmkyJBIDGLOjuWPwBgMKuXAXJFmr5dSg3LlKFz%2BWvH5%2Bxzc3dwVZcgvRoA%2Fro60pICvjf3v1mRrxfw33UGmi7wAJrNTxc9GP9U0DsSdBlGBRhdd6NiZXZMw90rogkdxqI7UdOB5YBOVwqGpMWv1Ii%2B30w%2F0fdQUyWMW5gOMTX2zK2HsRjYTsI2gShERbps9WsTyPjS0z11vG%2FlHPE0qtAfoemUyYF8KNJ1dxOOFSgY%2BL80HzRIRic2JWx9qTxelprtSwO%2Bx340W2rMKbB4vP1nFWBaGHzvbbms0WUWNVut7%2BEfQ%2B%2Fvo7T%2F5qola2L%2Fp827PgC7Fzt2Qn77SJM%2FVSnglpWPH0J6jdkxtQS4pzJU40Tr0wyq4Ya8yPUdYDHRyIAD2aZ1jqTFbVasNcfBtKy4%2B%2BtbAADq73BGlsr%2Fn5RUsWtnAYhm5ebWCzAL7VEvWVHicZ5WH9vhhtjs0po1TDnwdPIBjqkAaFIbkrlF%2F6kdny%2BGVbXopGZkBoOG8UD8%2BD05sE5AygLHCblxCQ6oeZleXBM%2BsA2UavsI2oTVOOB4%2BhO%2FpaBYBj5THMzG%2F3IvYQW9EErymwm69Iz%2B7%2FyPugJTVsaAKG0n9qFRLrHoj7F0jy3S4btq6rWdj1SMH03n3jMXZN4aY2MPDmpd0ZqhI%2FrFq3rRK%2BZung72B3MUtZfRt5zPzxjkALWSXjz&X-Amz-Signature=55089f491beb5b0a180b8f843c0c020be2d541d7e2e78084fabfaa34e44fef47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

