---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN5RANMY%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCGEq%2Bjcudng5uETDbAuoFMukIzvIvidz%2BciSrk3HsmjAIgYUqI3eF%2Bij31vJ5jKepRtd%2F6E4J%2BfieIxISFtdGoXcEq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNDD8kBxg%2FCqiGkHHSrcA9JRxbAqXwpYNOBmOZhcIFrNZsg5upPQZ6gKKYwcfRSJl3FvqXXw2aYkSPihjN1jj2UAl2ik1xg8GWbpSlE357LQykiM6qqRIzh3obZder%2BKu1Xm2a8OLvTeKPP9xwhu%2BmCQhzacZRT0g1%2F%2BM76HKu0%2BxIHSy7p8O0LH%2BSnaGXyL8wim06bWkcvtk7CBesXzmlvUZmd%2Bis223FakbjwjSL4uX9oWnFxOfhrKj8iPTzx%2BVP7Rm%2FGqXDfnQMY5d%2Fp5qaaQa4fV3FSyTWgLy4Ob7%2FO9n6%2BYcHmfQuuQGd7sJgyNGMr8ItyR5K2uTI3mBiK5w1puxDp1cdTO3wqNRrWYMsPgbV1ez7%2FznHkJXK%2BcPwLaNA1J%2B139iSSU0fnVRWVS5DYootXfx1d3g87uE435Qpx6EjfUDvqh%2FxjNXUHHguDOFPJBCb0COT%2BRadXPRVl4zhdkpUA5gVByXuoOGHei6N1lQVlMOJ1mEITF4n2mC98VBpUP4YLI1ohviVeUEtYtIYYeKSnQbbhZ%2BU4StAqs4mXR6wRmMnfR6cl8alX3PiZ2Fp0B4I%2FoXbZvgpW9thc15cMHkHqac4dTxEC17MKlweeSd59umi6MV7s65xnDYWeQRKkkq5260a0vblhPMNTTm8gGOqUB8EDHoVqd%2Fmhz%2FY%2FZc7K7Fh2Sk%2FMPBF1b5nWNL2gNbBAiKqG4amXm8qqxjEdzUMl3k11IV8uKrPXp2ydrMP7l%2FtWSRL%2BQZtmkBAsh4W6hHOkJMxsatw3HSNguWZdsREKEfwESlwbMzFkkuVcso5z2BH%2BwlcqEC4S4Dl7LopeW99W7IM6Iv%2FQS4aERPZQjNw4iZmeNiHbpHvfzu5m3MsddMPG%2BrZNc&X-Amz-Signature=3b43e3d8045114f112709188d358f3658ad5e8352eae25079de34f4d1ca2cd46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

