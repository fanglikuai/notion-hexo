---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3N6GOVQ%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQCkd03GgzgF9wSebhtPB5%2Fjy9JsZkPYgm0idlXdrfz6NgIgNnNXZAGv37T4SyuaKs4ZSO5s9Lw%2FHmEU4jSgEeIt1joq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDGMKn4i5Xy0i30qatSrcA34owsHcX509GFLMUAHzckgggAUg%2FQmX8w2e2Hhtv6%2BQr%2Fp8yVZHZNGEVesbPR%2BkUDUJgRK7dzBwEiku6S127ubjuCoe8GV466Vd8IJu3wqt%2B0SByQlX1bYTkkX0FHBup%2BNWCfKdLiyz4RWXe9ojH8yfp2WJpQ6sfQdlZF0DiRP1JSF20qtUiXJu3Lme%2BgjDlNy7RTIKiE3nB8dPWJjUGraw%2FdrLXwdYSqGoXJmddHWoLpXUnw2XRW%2FZe8c75H2mpK7jgey7vZDmk4YxSBWkxssmHCYkUZTki%2BYVZy%2FX073LdkVn1Qc4WHMhK2lyaSnvqB4DoDRa0nTTyxyWTyPhFQzvgqk8qoX5uMuAhgdbXBUsd99XbB%2FMBggzEoVVhBaPiVjSZJi9t74DJ9IRJ6o3RNAg9IUukDovHIAPh3QHLiP7bgnPHL%2FU6u3ywuLanUMkQkQ%2B7Ke5OKhUQ4YbdoPcsytNkhsyCWGgUoduPzHBT24emfJXHEa34C4iU%2FTD3z5FH2uiEJa2wq3JB2%2F4syV9z8PB4cgUFThONYW2JeBenRe6w9oJ6yALmw3ebKa48mxqhOz1Rjkxi%2Bdbu4ZmXOelr7amusgDGitR%2F139ydJ7HHnHz8dl4iLNZ9bO4xunMNmKgskGOqUBUpJU7rfyhRqSD0IrM0cduLjb7Hmn6EFVLQSioor%2BKn3ITAyOPy9Pf%2FM5dVUvQXP6o1mfB4QoUUXcFbMhMR9dWqskWMGBRn26p7z%2Btk%2B2svXlMHcsxFiaQnHG7bfoBa3WmOZWP%2ByM%2FM%2FY9sVpqR92rVVUIHg9o30OwX2TcD4a9lU2ntdXm9YtxwnGFUHMRxsUtY5aXoMsocJ%2BR1fcghxMH6idEpB%2F&X-Amz-Signature=c91441fa869846780fce5816ff1fc2721cb9d9aea5989eb69af21d738fbaf230&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

