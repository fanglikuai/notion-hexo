---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CL6VZYZ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICRe%2BHRFklbaHxIfnGdJCGxh0NUWI8Y4QPJHIvxV8a6EAiEAnSD%2BUc9auSfwUSqYDBaClDTwkrXpuhZGluAHrt1rnKIq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDLAhI9pY31hmgPJy4ircA3aaVI9VkvLsiPY%2B3MK0h%2Bt%2B%2BONunnDL2tC%2BMoj0RIHZgJVHw8WeZsTgi5ifKE1yAToVz6%2BvKIL3yficvlEN0xRN8VA2WTVN%2FRNaa59u4TxvLM5BlSdM2GvzpSTSCL7Wd17vFgp9tHuPsUaYvlAsxC56MLWmtzDhApTUWfe4MnAOsET1WbJaPDNjuMpIjowv9YYB1vhXoONUPgfJf5gEj42qn5phHcWlLDxrNCAdP9C0MBjJUo5RmmCmJ7Dv8ITzGcfvdakaudtNklmv0kqkFrdb%2Bsl6JheXFlTbYdaCW%2BA%2Bpjvj6tHlXcq6%2Bj9KYx52lX7WJFan135BIi6EyMSnJlzBE%2FDNX3CSpXAswZj7KTkHWInaq%2BtscW1R3FBbo7wUVhcI1oNZFxWKX3ACJSTmkkbvm2qwuTAKHIEuh2WiE%2FRQkE%2Bupc9OeBzJOcUbBojFh1Giz%2BjI65jCdA2kwwZWWiVK0yDj9K%2BvBZE9mymmJNa8UCEzZVLHoiF%2BfOGrjmLehzre%2B%2B2%2B1iHuWFklAMSGtvc%2Bs2wMVF0eseSwWXWP1pPSpiPgqKHWkRKpPvMsK81KhIjet1LGE0uae3VqxXw9iuXChZaYaxND7313OXXlJF5PwlvdBmQ6IPpMNYZLMPOEv8cGOqUBwpHrYDnPDGv61%2BobvSOQ49I%2FVtKKkHUNAKkbKVKTQb7cX7MLNTqgWdE3RO7T60bo8%2BThGuyEhDIxeWG%2B4DOersRkS09hL41dqQxvpvON0J0u6%2FScCycaxGC5tb3chdoWBvxb5uIzT%2FltuGUdmndGB%2BTfF5N5eEKfYLVOMfiitT37nzo7mE3G7AkM6VO%2Fz518zSNfgLYM4snCdz%2FZchdHJc3URmGz&X-Amz-Signature=8f9e96d996dba19f3a21ebae246bd3daf894601b11241938854bfa57f2e3cfe4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

