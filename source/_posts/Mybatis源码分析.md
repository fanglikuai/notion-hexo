---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTHIHCFJ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T080506Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIBoMu5KVi%2BReXz2ogZePUrcbyI7OjfI1HH9Xnbe5TTh3AiEAlSp9AiJTp8NeZyRkEvjvCXviji8Zui7B5FWHrW9XsMQqhgQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEb7woku28VFbTdDNCraAwp8kUcYTJO0xWCCA1wq1G7I%2FC1Z7QV7YO9uLFE7kf8f3KRqPOcYvjl5PhenNQWiFpTidk0njSbj%2B%2F7QcwpC34O2U4vthsej4ZrMxSHAbTASIC%2F3aFGD50VZb4ehGkzI4KeHzp3E1rpw7iLVZ7CBYuGTvwoqBkWaSdQ3eIRaNgE63ubYxH3Vi5xckgn5HPAA%2F4RbOX82RCm3yzQ4cJ0tWG4mgetGote7Y%2FybQdJpqk8mcafDwhjdqTxWDYWFyoKp0JhiDlIHw3JPBwx4A3%2Fs%2BdNSNiGv1TkHtMEzA6VAKhVG4mQjuTRd60CyCW4JT4tylIXoKxHoocvuR7TWVAC1CE8HHMVwoS5rZu%2BePLLJdRY8CM0eQrqT9YgjsHh5rqokUbOgVv5lDIOuTofsOwyxt8Y%2B1U6gMdcxhLnA25k6dU4ThlDlgxfAwph4FDprXWfqn5SiG2Gx54SXNgFpzae5WhWy%2FA4IrB0uFzW%2FvjFbRYyWUNNk9j4Bs1gX00%2BMNDFj3j3UrKLoSuqWr4wNvY3xfnPC7YYzLaesZurtlH2CF%2FOwhqXlmBHiz1YixLK0dCY9KzBORlSaEa1fQJC1OBQUhmoc1Y16Jn48%2FkX4rsh6dcvcT2vHM9iX1fYy9DC%2Bz9fHBjqlAXH%2FrJhcDq%2BCt7ueJmb%2BRCwlSkIIevriDc69IBwj2Z6wxRh66A%2Bus%2BNG0W23S8qIMpY7EHDQ2nTRLPFGOAEJyaxN5CGuh1x0fBzy55%2F1g2%2BR%2Fl4UsnaNhTVm0kGFh385bN5N15sInriLDn3f9FLEofsyOESNU1gFNv5cwB2m9GnLBA1iYNlOPHrdeEd90cy2TDAesnnEYLkCM1FzA8IWgrL0r8Y7NQ%3D%3D&X-Amz-Signature=76c256144a7cacdcf59383e3ba08168968193b1a3d07534d5a353310e44eb41a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

