---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QPREA7U%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQCn2qdos8vrKwJS48i0auyDltTFE%2FvqucL2cGnKFgMnUQIhANJlTMhlkGCuAilqzxcue3hBVoOsL%2FmZRb5Tj3e7pFmpKv8DCBkQABoMNjM3NDIzMTgzODA1IgwMEujScjVKC2sYMJ4q3AONXymAKiahWxvT2MOty%2FTwcgPS%2BM49tE9bDiITml%2BA5jTZGbxoxztOK3%2FMFj7s0vD%2BhqKSWLsKJhHhDIq6gonKqLEIboLFO5nBL6Cbi3LcSTMMD49qG0mI2BWlzlagW6afP5%2BceJwcAVGc66Oy2fc1TjVrB2yJzBvNiM%2Byg159JDK4ZTILTfSnVUMvN4GVWA5sP3GKfXJzysHWCGKQTAx3dMRCn6h%2BCJ0Uyk4Og8qeJ2drCWXyoXV2SL4wqBvj%2BuloUUUmuDO2PZQtGFZTz37fawz%2Bc7IAMwXATA46Z8o%2B84yWXG4f%2BG%2FF%2Bkt9axPLI7OQc7H6D%2FlQ0Q5jJciTVK70UHnKcF9lSku%2BzaeV2%2BgYFgst6anEHRbW93cOFrZsDSusTghYLnPPEzqZTT9HRFDHbpZTpLYVBou7C3SvzDm%2FeZ5OshIB8rkEx6r5yFosWngPVoTg26DIk4aoT480utxMXnKrmDg430p8rZIXM25J3Ifmox17JajOjYII6kR%2FHd0n%2FZpkZDBxgbfoxLwMcZeHnlBW1jACDbZaExfU%2BmqyqqTzLBNq%2Ben1t1C5x7Uu4VzA7TxE8%2F%2FLArLUmxs5gAhiR9Oj18nLrrlFK3nVcvnllZeoHLuQV9LnrWXMITD70N7HBjqkAep87lsCtnKihgzbauAEyFfuLiDomOIBHsUUkmKJ4TAoF658In4P%2BqTuIv7dSAeJqTrLRGURshkFzwXPbsAMl6iuDxD2l6m7Gg7iUfBwoUEvvj%2F8S7ic4veQcuW3odtYfsTSRHpWmgvpIcyAlAfopxWF7eVOYuLmhjI%2BHR9KfOTsrmE%2FRxPRpf%2F2%2FMJM5F97n2i80h73r7VIfzmYYsKy3asRxOb3&X-Amz-Signature=e342541ac99f9b1d88a869fe7f7211fcac80755bc4e1cb747b61d434410b19a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

