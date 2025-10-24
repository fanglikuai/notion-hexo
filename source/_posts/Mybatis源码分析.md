---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664SM6LKPK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDu4pf9jjgCbe4XhfYq8gv5OYTQbfhHx%2FI%2Fjg2n1yM05gIgWKYW18lg2SjujrJ7PnD0scl7MmiSQlu3YRtHGz2EyGIq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDDhQxaTCpXrHuDyegSrcA0iyfAxxjOwBJ5nRSVamU7XvJihGNkTJr%2BosQyCLmyv%2BIM%2FCyALqh%2BDuccX5fiCx1lp4tFkV34Eno0MhLlH3XcubXhaWcuUa780SQfngpRmQH34q5NtohNBRW65KV%2B0fjXr%2F6RIkLnNxgu%2FaNLh%2F9EGZq%2BYdSMjUd5%2BGdjmziexBE4C2yvuVRiisFNjak8xWaaluDB3Z%2FUa0SP6iEzBO%2FxUhswRMmRm3y1XhQCdPH%2B68GLBFRqXR2YG5J1LxJaSDPAbDsXzD5VnT029ffmwxaJNcUM7mTA34z5in8t0wCkXHVuXaoqY%2FEnlKO1KqIQ1mNyXcsR2wgmfmnnVN%2BINyRaZeFbs9JFX6Dp6eNLCIzgf2PAUzGsdfcTBtvRWQEnZkH1xMFx2EhyfhOcPdzdvbWet9cB6qPotD5rQOMme09cJDiHsjWG%2FV24Yw4ZcG5JObRzT0JLh39pkmkMi6lRxD5Kc1xvaOZHwrtwvoEjK%2FIob5HBWO8i52nA%2BH2wfUVEK5NLxoe9fA%2FAYWstfENFq17L2gocfXRBaIgFgGr5qSkLUhDxZburqdmiNRTK%2BqUljbNiiovKLsj7Y5jrfupUPGHO9cBonq5RH7NrxZKqcARAxSIlara11kjMWM1H81MMeL7McGOqUB0uJFZml9CxzU0C9WCsd0fn1T8jced%2B4XtTP%2FNGatp%2Bx%2BseEffmkBlzmDFLJKb18SIRt1VWPSedpKeCPn6kPA9UQjJNA%2F5p7USFVSo8tiwoGPZutqyYyP6ewT%2FMN8TgWICyQG4nFlYbz62gfk88UwqhEEESqztLbUrzem80YUXlBJ8ldUZ0MuSxBZGCHrPNp%2BsJlFjrruKMnCK8EPJHgg9HIq0I2v&X-Amz-Signature=a627c204a7ead2bc094dbc554f494a6916952afdd69a64aa276bcfd7ecefcae6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

