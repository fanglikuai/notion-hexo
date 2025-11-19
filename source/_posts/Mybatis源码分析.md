---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KV3HOKX%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIH3QdDxT6Y8u782F0Z4%2BtEo3R8ECwkQ9drSwx5mp9tvLAiA5Hz6hc%2BUJzg%2BmmuIFlIvUrxLKgEz1nIcLnFBfCFpM5CqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP7oj77JTNHFqALu7KtwDl9qIRux2BGR2TsTKVS2prR8ZjHJEDszhS05V4q0MYDPznRiD5DBPsEWkyoxJ7feQt5niMqHuBya%2FESplQsUEVQP6w6DdnmB2F7cNCQzQwKHrdlBdFyFJyfqhnUfVTriGTsA9vZ4s9WQCawgbHSA7LzpkluXmC3x%2BsOG9lnCmtkTx%2FXNmkBapCsrjwF7dvN0BuXzWTdiiUQ6AmlKxc3571JV4iyXR7S7F3bvfs4K%2Fe81zdDfWdXHjYB15Ny7Sk851CyyYuXpVqKZgnrI%2FVvJ2MS1RYjnLL9T1ahCoxaZPv2oX1vphdboowFo0B55T2nWZKPvfFPx7t6L5j1MVA3jO1abJWPY9%2FFkEKh4KGDUbFFtxk63g2atLHQjItnkm1WXa7uDJ3MBeVCTT5Fg7rNXs4L%2FcnWMzrvUg6Ho8mXQ1eyY9qyGEP3c2d70NV4WXJaQx95Di%2BCqpIEvmZu6St4trQ%2Fq7r2fiykq9CMmtZrY20rWYwo3sIt6cjZpiYb3SJD8PUqv5s4zqyORPaE%2BsPSyvV0oHcTicGg42YC4ChPuf9VLF9QBIkpwISAFpJS6%2BChSdPDZAt2MT9y%2FNcTd%2F%2BWIhHqGC8DhzoerxK%2FO7bp1iEpBi8Eh88T6wIPrvoCUw79T0yAY6pgHmhKzap%2FNpBMJAvjeWP%2Besldti%2B7cvvyosZKP%2BJDScdHwIsVDXXJOCdDoBefKo9n2WvCqeTUCBfzi4yVYDK7MaFx0HWPCLLdM06MrXFJljXbepWFMXdrlmuLj5x0MNgw9jyRn29H3kQIyx1D3yFxC%2FHoHVFDMEz7ptGmowLPnwKRW3JtRVS7A%2F9AKkt11bZlDsmgKyoYFdYPzYYzOmwvWPck9OlO7%2F&X-Amz-Signature=9945b825ad58ced2abdbdf4563ad2bb3b0004b73ea8a0968e4993b26191bccf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

