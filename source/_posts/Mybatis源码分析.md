---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VE5EXDVQ%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIBKwC97a%2BFyV1MwKc%2BoTggxOCRtcgWtUKEJPnxEMilNDAiByX1AaJZus6F1DdxLtJ0l9wV7e6yF4fvOSQowdZYk%2FDCqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FyPoTzBcm7MN10ZpKtwDHzEkjfvrOd7Ota2es79b43KKO%2F3FnDfsFJEfzqJxwjxy2S1xpfRTCdu51qLwtJvPTuNmcq8aiLO%2BPlevc91Kb1mXP7KFQNYShfRL%2Bu7WMMJCJQoW%2FUGEvqpfLAh5B0F%2FuNy2K5WNY0m9VZud9sv9Z5GQy282a%2FhpwO4VR6kTd7522Jf41XjSkipYI2eZcH1FKdScQA26vQOAOQU9RrE111SELvm3wXoi7B7c4rbnnmENSNPbXt9%2BM8IewmAAIpBZormD0U10YkanSVlTizrxWgwJxV%2BwxXLKrbiV3ZOfsYW1V%2BnwMz4prdwvXbewPRfnTJsRGBPKJRHgFa5Sn3yJd5j6DnzOnNsS6P9NPJ9OXiUVbLcJ3CTfsG3SBntOpy3ycYCIo8ecPGehk8rq2Jn2Nu0lb%2BagJnlvPlxyxg2Vgj7rHzmbjAibqmr3LBUlDuy%2BjDeRF3oqv4BoTWCiMAoVbSWm2F7QpWsLvRqFpDTJC%2FR%2FV7b9%2FFEogAehvzv%2BpVR5x2CZ98NB2%2BlYAmEAKHguWbQe9zIMMWNuhWPzRcNwmPk6GqEV5KcPVIrFO%2F%2BmTUINgG0%2Bfq2hPn5CLoVYXopfMmBUPO49N3JPuAUSHv3%2B84DY14GS2zCi1AOf%2F0kw%2BtSrxgY6pgE7g%2BGwyQ8mieGacKq2SjuvvEI0yRSVd3WX0JlsPE%2FL%2B2oZXjUA1QUE5rCRa3Xrjj6vTNT5qsZgQy3rB2mEhd0qShKGUsmCgaYfQ0CQIn9UTGBnIWLPleg%2BgtQuEtWi3Z4Uyhah1e%2BFGVHBur%2B4MSXl393tb1QCnUZS0iJvYF7kXNMseBUjSdQfGq5r1upJ9uHfrLCrv63sv8bD2DUdf3SpcZcgrZoz&X-Amz-Signature=3ab3cb31c0e27707959b03ef9bb5986aa98da0b4cee6d97f696d6a62252ef72c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

