---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJL43OOZ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCID8U3af0iJIuR3kk2VFg2grw%2B3i5LmBWIw89VN%2FZ1iWGAiBJGabjtkovF5FpX10dcSeBZ6Qkt4MgjdDMBKy6KWxRnCqIBAjG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNlUH3tByurJUzaf1KtwD5RupOFiYnfe%2BEIiJf3Nt2ZPTGIVBKWKi7uDyiuDaqAWVgR72ITpnUX8aaAw%2FcwTxv8F9ePvspAy7nUHWylhm%2B1q0faHChR5Lal5FvROy6hKj1S3x7k0wmIUP3YIuvMv2tno4dIKjEM40hDLA5xr0U5EgxmogHYK9uy3Ad%2FHuIPbjwbXODVUYrGybPKd%2FMwb5C0WL4oBFCHYHPubcDjisrggCnib0P6ASAe47B1Xva9BdAebLriSxcg1aNzxiwq4RJrZH2L1vAHvw%2FML6oW%2B5xZFSSKk%2BYiBHKu2BOA0EKtmV5ZJ%2Bq29xn5oYtEdlFOgprp4NEAB87jhNeGKuNzJiQdh%2FgUDNyRjAmiAyHW4yNNYfMExkxqJJu5wrDU5GxI4ahEc2fnMW6wbgiArdl98pg2bRRH5hvpJkmgtZnG6ygJe3xsRBwvh3svgwrZcqNXFf4r4xdhpTkgYu7byfbCOh2AR%2FHfk6aJ5n97xfbteqRYcp3pCmA09wRZ1rVeLakJqhpFhpAqup3zOPtc%2FgBMkpkHS4ZRNbmB0g%2BUXgSJo8byUxbITOyDhfY%2Fudzs7VdAeA2PtazchA1AkFSVsH3nVXzOZEh3PNiSMuza3fCyZzsU5pllTmk1BQyEtf9QAw%2FbnmxgY6pgG1tWIMsXuP80n%2BlIZlZBBycX2Z68RIbhrZA1WS4bgjHTucMK3J8REwFGG%2FKUWeFzaLmPJoySMzVt5QyrmNh426Bg593lWju84fx%2F2qdJYBL6FVvB8BLLb634hAnrL3R41uBnBT4NHb%2B8bB%2BR7SFipn7s%2FGKJ1R%2FU7UYJUZcK0CXRDipJAOG33g1%2FTJXceyLc9%2F58stWJJsj19ebHxO2t8XGHb6aQ0O&X-Amz-Signature=8c06c18a5851afc8fbaac66b522081cac7285a57a58b2ce9a1869d49f0a7629e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

