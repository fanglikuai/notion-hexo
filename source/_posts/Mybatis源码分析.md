---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDVUWOR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQCEzF94%2BbJ1CZbHs2uecEDLB%2BvhEUQ%2BeiepnFgXBaIrwQIgK2PnGuBn48KSrDh1z2aI3UQimlafJaoFAncw10MWpnkq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDIDsifyxpVQe4UG4iircA8ZdTWpJexKfbeKRTVPqQxK%2Be5AtzrGHkAjETSqm61FG6PoSYJFIMsQ9uLmIGh%2BtCUdYLMwlIG%2Bjfu652mAvgoOBPhTbQ5AOCA5ruyqfgQN0PdxRlPmapakFk0%2F0ZKe9Iw6a6AtoJ%2BxCfemER3Qg1KP0d7ZH3kiAF6xVeSoZvMBzDsud4E%2BQC%2Bej8Bvfr1%2FlHEyU9a4DxrUULJcpNPXQz5oVHmJmupLrgemf%2BdEXwulQ%2BlhiAQezGcHgv1HB7gdVwRwHW%2BeJeIRi77K2Svem5tCtme2xOWy%2BhhAiu9ntc%2FqinMXDZoSbhuXxHN0o4pSWNxjb%2Fg8ohT%2FPwLH3eOX%2FhgbsyNz2t0q0UsZlRHEI328kIpToeJnT4dzEWIQCQ5Ih3KVbxQi6Yz%2Ful31QoU%2FZsP2zKc3wQiUU%2FkWtS1r2rL8hrWog%2BLIVQRt0SnE26hkQgwxwBNSxDIPkqT4EL3fIgkVOQ1FjV240Bru%2FDng8p%2FfQbGaQKFWU6LO6TcZQ0bY%2BT5Q7Q%2B8DDNwh3HlvGqJjZi51EHKV1Rwh0YSLEBBmJWlDDo%2F2b5wzQnBrjkQJov%2BquhJmzQ66asdw5G8NR7XZ9%2BFRD5gXojq3IXXzHWfbD%2FBXcOS7gBoYuWzKKV00MJ%2B6rMcGOqUBqQPUC1%2FmmhA2m2K%2FTIJ%2FI621d34zW3oj9gyZ4VMsIFWh3m%2FSXWg6SgLK%2BaHOtJ1K2h4WiQ6m6lN5peTRVwb3kuLKTsSrrM49dFTWKuEv8z7SQWOvtCvGKqFmdGFIGKydWbhlOWWaQkEngjsY%2Bir%2FqNZJ%2BnQKn%2BCZ7pg1KJJPLLciW%2BbSYQAg8kjb7lShJyTvoBEflsxu1U9Vwh2OwSBmiaC8tx1I&X-Amz-Signature=6fc75490e0ecd1ef5bc47e65ce92a38295fe438772d1504b4d6b1b53c2c5de84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

