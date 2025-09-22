---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKYAQJGR%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC4igPWfU9t78xsuvfooNK%2BczfrSC%2F95Q%2FHtupgFx55tAiA%2FSgHQ4BvTdODy%2BeagYEdYM7QoSqrCGJK1jzMGl6CSVCr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMuBDWPzYjP%2BmkeHOhKtwD5awBxO9Xmkl%2FijmquThX8WHzNy3JLSS4S%2BxHcqN6GZ6qrFBNI%2FtrKsvCRq%2F84XTm5OGecATkYUL6G9fwptWXYJD%2FG5%2Fy2wm8CxtCxeYU%2FRb1jhCLgB0pVBQ2YaSUiQiXqOQpbjlHEjf7DYAfMtUVQ0C%2ByZ1r2KVVNyW9TseGAAg0XxulQY0f6uQm%2BxC7J5LQma7ahm531sQigiLh%2BYrPjeAxqrihXmcTeD0PzM2WhDc3xhOL5TuAdSJcRffNhrHzKrkXUavNx6btLewNo6VpIj%2FwfIrTd4sz27VM7eugRjlUMKdlNZ1buJNLgN7harSrnJPstNWBxxoXTvwv%2Bm2R6ulttwFAAV2Xk3n91LO6LULbMSSEB2c2pPrbb3BNOqKqHqD7ZGIvcBubvKYJt8IexcYtl2qoI3bKNCJ0mgASxNh0YLjkFALbcyFraHqh%2F2m%2FMyF74eX15EHy4TO2dhHsjC2oXHiO7R6sqH2Brjd4c5lc4RH51qVLj4bi89kaHV%2FbcernLHs1Fzo60qaLzt%2FVD8HFnv1acPFVkBTtFjnvCpod%2BLGAcAQI7eVPrVF5ngaJ%2BesaDv0lmtCdnAUVJ7Mj51bya589lUGh1PHnq5jcRB8nmz8SojTdq56Hvv0wre%2FDxgY6pgH8J4isM%2FTyY5oZv4jahBnYCRrgr0StCm%2BNCATjOUG9woDDnIeNtcAs1NpG%2Fi0U8iikXXwH3PvpHMqQLW67hfKNgIMYxTkR8FD5HriWCsB3Fl8OKzb1gbqPrXGeA0bnzeMdS6GMFdWLt8rnavIJ5eH90aoGIzVAoUBrBfcxiEiN6CK9J671hkExOzE6fdUmn5wI96o%2BXkHm9KvJ3GQsi7ME2uY8%2F3%2B6&X-Amz-Signature=6433106d6af88500d4f2793d340a2f13a9221da33f9ce737731ae6a47e6d1313&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

