---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YA6VRMWJ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClWOATPUdDaAFqmibBPQb9cVlf%2FhN4R9AKVOUTGd261gIhALIhalhGtH%2FbrisbjthrE6SWbtL%2FXWoRLo1SC4dD1qX3KogECMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxuu4V4joSwCS1WPXsq3APD1wQIeHeIXjtUXkBJGbjl2AJ4mEMrplOSHhA26JBP4xEY8iNOhAOHk7ydEhBfJwttUkHpjq9grtQi5SibGhzdOckjLD%2Bdir3Jv2t1ZJmfXiIIqW%2F1u7%2BeqVeaKlw87PoKszzODp%2BVSqOxb4Q%2F3T0NHgYCwz5Mu7g79o3Ja7qdvsEU1oVTKz6ZkqYk2nOtKFew26eom%2FFKPuEEYicDYcejRacbgj4PnIwr0r6BFZc3ciUYTFIrE53jiCNvY78z9dGNK%2FEzuDWKZkrM%2FATpRaiq8y0%2BVcZy92Wte16O0TRqQsyM73a2KaZInb203xvRaBDplHNFZzW%2BIgjgs5PMOLbiS%2Bn2tKJcH2zLGiu3pg2zRVkFIi%2BmQowQageGrBMeIO2oV9RHtMs1jzOiUT%2ByS%2BaambgCIqnMWAmM9Y%2FYXZl4IIvhVSlGUsTcHyrpzDS4gzh36LcPc3u6CZKt3ixZEJEMG0J9Ud7du2Jv%2B1Y5IBcyrz%2BqXERy5RcJ3T%2FhRvFzl9JePpu8RTxpsdp8%2B1Bgz%2FuOfu%2F9UvTMRm%2B3Uk9WYiGMTPVFWJwMeO4gPdeiqDjqQgCNODfk0gue3otQlwEu2PfVws8nf%2BFbVNzEo1fEUsvzMrsYKWarIl08v46w1jC8wLnIBjqkAR2dSxIMoWg7Z%2F%2FpgiJpW1xRdFEjlKNwV54C4B43VnhY4zXIurQl%2FOvJoI1ZXIkJyV2pm3nQ1KeOwfdvSs5u8X9rwdaZl2tO0toRqx%2B5IGlPVtfSgx617UN9Ze48apgwQ3MBc0oaR%2B68iLoaeh6X6cdK8TvzkshuHGS8%2FVh4eahN4rUvDE3fBfwYngn7hJH4uzo2oFr9dR%2BCPbYGSzntrh8u43ad&X-Amz-Signature=055799da8de2bc31dec2609b0b5b6e0939ec7b27bf47662ebb25cb50f7f51b02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

