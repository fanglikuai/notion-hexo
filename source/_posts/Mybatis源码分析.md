---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SJL4URA%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCmSwusZmUbuFTmMwF1qwchWCNTe5Wtg0ZSUH0pKNMuTAIgYRY7QJBL26G%2FGyfmR0Vhvx7DIS4tOQqEpVAZCwYx4XgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOxBqZYd%2BzeeM2iJcircA51b0vYWWhaYkD7PoAP%2BaeRa2J2h4JeTLl%2BKhTHVvS2afXlLdJcTNi01sdYxvnGPfMR03kN9D24t4IIVv2Le%2F4zROpcmxDlvKhBhnuu3ze5hHseEZZFp2F8SnnjnMDdIcMZqyghSWuDY%2FAXJX11BZOQxwJ7Q1AS2nKdcs%2BTO%2F9VekSWefseftUWe7NCJ2JeT7HiUxx96qxVqux2UJQVt3fciKwkOEWaaFYhaO7gz6GhY8Jqq2dQAsd%2FYfgTJTin8DiZc%2BIMFEBJ40XfvJVIkC%2BHyV%2BVBNW1DkoPlFXfEMSeJFtaU4cShVcqMgwCMFxWrGbd5RYM7J1Tod5t5N0Eh36QXG5hweNJi2san2YA7MBC7%2Fs7E5rKVm39Cibly1ca3tsLZXLTH%2FNpeNK5UEqLT%2BkvuUed%2BPhEmTsNGr%2F1iJt2AQEtuHJmMAs5E0qHsLRmbJxnDQStTVJV2UQcVLxsrquDxh%2FAH8VtM1%2BBBGqs86hR76KncTohL7s3SwrEMs0CDkOKQl1%2BudEiYh%2FhwbhAYUgZ6eo1Q17nd3JJDClitoRwwerd7SK%2FSCIOSToPb2o2nbYf0fXhW9g7yF9%2BeAxudZ7GUnGZOLKELZnQ4LAiqLFBWnByBSO%2BRFfJLd1MNMNaThcgGOqUBQW%2BvqPtv6A93LFSKH5DS6m0SjBtRwx62iico02Qp4z9GugHu1%2BjPoU4vxJN02td0xx7FA6ba5njP4NEvdKmijbtG0MZI1s7aOZ30vZo0kzklNdMBW4ors4gnRLeUbfx0e%2BCQAybDWEJ0azeNadn%2BChgZDgq9gW8e0oxvYyd1xvLPMwaVAbuEuZyipV50OzOlsgFp8KuSQU74o96g5patjTccbb2c&X-Amz-Signature=9831e814c6f3a8c87a9a5e96c2012d5a3bed5909bb313c8da7477bd53e9af84b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

