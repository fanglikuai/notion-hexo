---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTDWRFU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHArOKYJgB8noV1fLGKTHpETAL%2F20H2ojYN4EWHlEAvGAiArgnehEaq6osthLAVr%2F1CqNiTAch3Au0NqB0PHGoI0%2Fyr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMaVTfrz%2BcuTeLUA9tKtwDhk00AmQJ2XE1TdFrebWpdbkEBZzarr0dBz7Ga3oWuqd8nVACGzkYhvkwIdJZamtP6rbWr6KnJKMgJa%2B4ccaqAwb%2BY%2FhXDE7OVyYLHCtE9S%2BgNJjohQjl3nH1Inp91zWT5vNzBUxMgMqzKDQr3gAcriuP5slEzJnyWXX5dp1fpldd9f2agb4CAnISWVE89PzupNYAPkNlufaEo7ggm0l71Zkk4M05ov%2FKlFZWbFjXcgvSZQ7vFkvRWhM1Lmm4BJuPFwGEuy0%2FMdzwyBs52cAtTRAWM9VrysLSg4iSlGBhvYQA8uaXr8fwmHBzGBuKhXiZRjHOFp9CGUfes9EKj9DJibC5HCNxp85sKUdKQhvYayXnxNrSI%2B4IUMY0sbQ5T2xkS2R11xnbYP2apJwYlvSc8zuLEizoMXIEjawg1vgrvLZFn5Jza6cxV8zHFaOV1JS2EHOWi1fY0GauMzShoIaRFrHdeXCWierX%2F5fsjdrVVjktheyi5UehVNbYuuQPMgN047TlV8%2BdEAVjZFiovu%2F52PTIb%2BO61nDqRySL%2BozxzId%2Bog6BMn%2B1TcmjFwnEbzKe9xtHpKH0Xpx%2FE48nDz7gsQ32NbTTGmvIZ%2BL1H6RCAgQuhYzhkVulZs0oGF4wnujRxgY6pgHJ0igsJ49t8Gk34WDCU82isTzOhcJ28s%2BWtjBJhn2UE%2FfEITiTwHwxSGonttFfL8zwXVe8Ukz6KP4nq6pqYN8ctQffguR0%2BV%2BU17EtCyS%2BBjggACY6eF05H0dMYUeoiiT5BGz1%2BQxnw70cORcthecDlQXYmyt%2BrZa7Nw%2FITCF0dkNojj2gF6ZpU1zPET4jpvtiOjWY9X4peh5DauWP5slkvGIVMZAk&X-Amz-Signature=8697264929be912c299f29afb1ad2eecbb70f2098b3b01d0de372becf3159d31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

