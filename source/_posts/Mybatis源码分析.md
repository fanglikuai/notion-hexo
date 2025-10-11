---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCMKTKO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIGsBUnJjE6kMYAgltCfDkf6vf9GRemXVM8ElCDi1m8nAAiEA2JT99P4GN6vElkz1oGslhf%2F3ve3vekI8w7Hbs0Fs%2Fhsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDGkxngAW0zQUbrmdsyrcA6XD1RVPey%2FR8DXKtZIm705%2FBuat3TGo%2FcG2punQbQGRmy28nfSNLOfCCVU2BAP%2F5cGNEj5MAEr1VAfvmV86AvsA0kXw1ggp2VHH3I0A%2Bv3Fu3biqEk44gg4aGcz7d3UXXeUda2t29qrUADLagTqIn6FkX8BiB0NYJlsJzXPIbnr%2FILCy387FwGk4gNu%2BzNLO2i%2Bzvb0xY5WYWHsgXI6fooHjtIMic97D7TyVnrsn0Avk5A8uG%2BFsvN0HV0MMUqMUmGsaV9yrbm1BPAKBuBmfFzz2pxFlmrbRPs1Q70e5LIcAp1iizqiCcVp%2BPh3TG9apphoZQS45VJOAKBXng6HaiXMDb7UiHtsRCB6yRhFZQ5r4zKNa0F6%2FChcZErX%2FpjsNDDSxHE0xFf43z%2FDrr8K%2FB%2FRtVDmUym2gVgyomAQv0acvL3mfQu5KOYgdgUXs2G2QHZ1snDUM9DvYrZkWiJ%2BU8bjX5Kdn5qb%2BLvQ41WZdnHT2MElxALXpPTkkrMtp6dWq3E2aMsJ2HEzKoMT5Bz9XC7VvXKGbGibF8%2FpKuYXPOzP5Pemp1x4ypnZaCa4nJbY0JY1nbfVzJ17%2FZP2w6Jpu0fd3hKYQ8EdXC9cISpWBikt%2BUPycZBbozbaHdXmMJ%2BlqccGOqUBiNW%2FtuqVdzISyjgXzG%2BYMWsvi%2Bn8t9EYn28Gl29vM41y9w9gOMUpBZO4c0HUhhP4ZHrMLGF0uO%2FqVVU8oq8Q654KzLDbA%2BTm59ng7d%2FWZS4wdjEVbQ3gHnJwZCTIvUIvchV0yvQ4x21DW74tsSBryO6i3fcFgXewZX5eGDInFMCtjTQdBaxkiKTndRaUfUslSZLVs30hU%2Fo1Fqeh7shEJYpaxmrH&X-Amz-Signature=fbb8f5658ac829ad04c2fbc25c8649e3d054e96a8b6dafe012387adbf5e96957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

