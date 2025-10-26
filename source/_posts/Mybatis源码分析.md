---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVOUVVWJ%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA5Z3kQhjw%2BRXZo%2FZm5NHp41duiWOkAAVRBkAjqfBmgqAiEA4biouI%2BBuoaKrzc3Flcw2MzIWbxIGq7kg0fL0L2nHFoqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwoIu4KxU8u2F2vVircA%2F3kWqTVTTJ7OtAH%2F6Rrex%2BN1uAtaNUy0OMDdnT20aeMnFlP8TQKpQ%2BwVHex2XcbZ9KwklKoXE1oMFNx8HgbHU0bD0pSsBqad4FchzSni0%2BeyZkcGVqwlcr0gWGCpBPbEwAjNC4MyW36L55T2zSZ%2Bb%2BHlajsViyYA8MO1KykH4TDt5NbKr8UeAzecJy4uLzmvL8PacoBzFrF3QKXOwlR2NJ11Y7PQ19LjVHZKrNlU5PmGJQvR1ioticybMOhSYXGatyXvbEPuCJ9Lk9iSY9voWsul93GN7%2Fiz5jUV%2FcAFutggT35GedHZpcmUFGvbXo9ON2Dc9WVnidfvNDMtvnWUdzD0Amepni5p76h91UrUf1RS1Laa3f49AejHsL8xdPyL52QTCynAt7LgB8pvr7deqL81XeiSSyKs1EABrEjbIcU7Kl2TkfF8PcGUGKSJ6sDzv9DjmBK31qc81z%2BPvwpi20%2BRyhBuWEXasXrdishqWP0CRzJyiv%2FfvKX7bICmyO0RxSaX8usAC4ehTKtVZbOQA2XNKanip%2BHcmBgGjNjrod8pm0Z3%2BVdVYwKtchEu5fk5H8PznFzM8aH%2Fe9N82l0mOivCs6lu3EmB5r%2FYvKzLx%2FU0Ofiu3JJaSSgPwaaMPes%2BccGOqUBIDBAj%2BSyHqWzira6mt9As4rUoolNYA3Rzmce9upY1KDgS%2F70Q%2FssfkYVZNX7nx0Hp8%2BGy5FYyadL2S89z2wahVy9ql91WOoHmcGYWUcwAQ819lyo%2F0PzKspTM8W5xXqryUBYoOAMmo0Q2xc3MJ69RzO0wIq8FBdOEwJT9uVdGd%2FDJrJgL74XC7lyP7WchWLwwjhfSw23jTZadHwAKYFRcqM13xbi&X-Amz-Signature=a229c93f3223503de39315226eaa8c6da95273fce3c5ad033fa8e2040c9ff093&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

