---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XX3SHS5J%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCQbiRr33xUOySsyirHcossQOhHY%2Fvk%2FxIBPr4PTpc9eQIgI3LsWRlqK3u7pndKTwrRcpAnhQAQcpppaghqAIyjUhgq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDGaDToC9qBMgjBFolircA3PRCDZP7Rz970NZrLuGu%2FezNtKHXt5FVyKX%2BIiahyKtF4pm2hh2ylkDgoGkE3pj3OAaljCojy%2BeUyGpMU04obwr%2BeJqbzWU1MAn%2BTVs%2FbzNhlLlkSHwuMNKoX9nnhRZUD09LR%2BufY2G2yimWXLBTnEAuyE%2B4qFk%2B1CjGoij0drlIjub7vh93uxouu81oU6NJaaBPmXeOBcu7Av3hUQm174Q847JaN%2BPXXxnHTMavI3mFYpEc329Wn4Jz49NxWak9gZ5%2BWRWL8lYXOblIvSpWzSBS3%2B964py9drPe9WPoUjAqBPStYLDWFjusFA6I7XGMJfxFomYL2Aer0uI%2Fmmuan3xPrlS3FAEh4PWLBdFD%2BV47YM2w1OvUCyJ%2FkoLj%2FjJKZMXLFwU5bnF4uyqJS5MJ3gck16hI4TkktKE4jeZd1%2F7oHVYJRkQVL0Dcukfu2Q8D8OxW9pfiGN6tqxg79MsBbtH0cT1zAu6gGuNRJwWNuRtEHeztOmRpH1ad3fHZ2gRpq1vA8HZ6KVUHy%2BZM%2BtZbnlhN3M0AOa4qIxAtTglhSG9KoyiXPc43F5itPSBi%2FpzcmjR%2Bt4nVbczwowH4PBVcJ1ghDJKeGJcviX4q7PNpJbtseDAJFNuTWmk00GeMNmrjskGOqUBj9d6WzRc0PTWPGesXPpz3I9%2BXEqzN%2FNLBttrEz2VIWzCqWcYv%2BfuFDitDQOVliVT85a9o8sOLhb%2BjcxwEYQya8o4levKnXdiAfL69ZGaKNqC0YvE8r1fcICSkLc7uTw%2BHaS8LnWKY5UZzHjOYMwplcBb6atY8XYRndPoVydPTzZatX5jvhPwuygsZg%2BMU1%2Bej0llr3aLtlwlKI9J0ck8SvOaYtmU&X-Amz-Signature=fbfdfa523d5b87aaef072b7798df69e84dd17fe279ecda961e5538bebcfb6f9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

