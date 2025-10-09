---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHYLSSTH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T150209Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQC1XokbtfcRcwckDO0T%2FeioUCw0hNEW4qRuspStP7cMPQIgWsdHKicq6u7X9VJxX3Skf80s4oZ6ynC4H%2ByvGniA7%2FMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ%2FYw%2BLvArwJY3%2FnjircA5%2BkNdraoubjYSua%2Bp%2B9DlTvF7MEKh4cdceg8HgXgUW1I3xpzUMNawAfLPQlmOayaZNrGaBGY7sBX1PMMaz0o8NkjhScaHFf970vaXkqhaanoZG5w9LorPw6zusITnHx1%2FvTwJcrzPe8mnT2jT7kQJMFtxESiEXHJL5yKOkrbWVzvmbKpaF3tGWmgdRIOa8H7oK%2FlRwGOSUBcJzP2pplsvWgRGo%2BCBrxoONFcKXsLTGdFEXJxfGZYEQ8cyfgRLO5qxYEY4ZjI%2BWIWhV7NhN2%2FiDvrbjsA7Cate5Hl5XY4G0we%2BN%2FXDgHY4UDu60g%2BY9dPEqbc0OWOtoF2bVnIVXWUnSBCywq%2BAGTVMqQBN%2BqpCyYTv1cb8v4iRpW2IeVq3BmkcKFQ4EWjl4e%2FvUgO0xA2GH5IpCptSrrbgdFNWL%2BZ4%2FEgIdAljk%2BO9MZm%2F2EQNEtW3yAT7WTzlf6tg%2But34IHYsyWm0YTw90eUFLpfbuPiPvn04W80L6vjXDMiFRaH8VjRQ8ZKIJkAmqlnIDQ2HLmAcSe82Bfx%2BGNNQMjxcoY08ePO8KbNfu%2BNLtRhUD8Y0ey%2BGSubfP0Qoky2pc%2FVzvpy1AXfh2BgVpKIumpjwzT5XWT1OytxTQ66ZrDPmQMIiSn8cGOqUBcQNNT6DP31ETkMYaJSZ9MgfY5VnqNTtdLzZ15V5vElj%2FNmzAW%2FEMf4Z5lCIXLBLowYejJrLe2jf5Y5R8Jd%2BwFEy6DrRcHliUcnCwTlVQGcZYUuKaU1hHbaSLIj3%2BhNt3nGb20Cu54xAudtwMOoKePTbZH%2FtZL%2FAtJ6Sq5yuoYb9Ce9eIDXRUzSpkeX6XxYs0p%2FASZCVJiCaigUzoibNEEWQ2mrU3&X-Amz-Signature=770a5e214ce132d03de47e5582d07336576488fabf8f549ae6b0222347dc02fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

