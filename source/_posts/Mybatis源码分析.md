---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LBA3QYN%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T183042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJGMEQCIHOeh7XpkTxnoOGyKdf%2FfXgkFYK%2FdNrXl3oI31eYrXsfAiA0QrgXJRqpTBQE5J4HROSNj9UOGyieUloRoVI09KNyLCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMrkFAoH40cv%2Bpy9VTKtwDX%2F0ffF%2FA79zZ75pSe7hvCOH6iGXD4Ra89RjrjXC%2FEQmzcsEjAVRMNuEa4F9EUkCNnMSVJFAx9S42Yk1FMKmjEfKnaab0139Ak%2FQR%2F5BdIW822fUBTYfoDTxqs77RemT7MElJpKw%2BStRrBRDozjEJ97guZxk5hO%2F3uohiNjPVbgqhwkCIJt7mYGIizOcyFPQSAZPX%2FP8eZX40NZYr0C09pRQ8V90SKUjvWJwtbWgtwt9GP2GZbwwGGPf0lf6jaDxbb1jO8xAP3On%2BQjWGPL3gLP7mdfWgaRY0ljDyAFQ%2FRTEEd2yXqO7E7TTdaN5cFSfIUpIY228JfBKdnjRkw0MeTPyZ3jiw7A6XOLoCPFoaDHmiuLrLNyYaoH4KfPtoDd0L7U9KDly%2F8abjpB%2BaJRSXhnYEz%2By%2FklCJi3cbCsxJ8fIV5aSGWi5P8kQv0MZ7hITYWGd1%2B3qMR%2BOcX%2Fb6XARBGGpcC4b%2BFT7h%2Ftf6ggXPJ%2F9OfMLYksQkwbWbdo8BCnvKJCBQJ1cpr6OabyUx2yElNFDvhPdj%2FI1OjKZUP4cRepZkWansO5ORK5gfB83CmK9Qtbzz5MJWDmbQXnOEs4usTTtVRtE5M76Cp1%2B523kpOE7Qw4q18yMHsoA7SEIwr4qhxgY6pgFh1oROvIznyl5360xBdwiC00mzwqHMwsrFZ4s2WMrgkl5XItkyuVqxE82WVzcE1Ob21PKpZyZ%2FrADQzUtxpjyYMQWPiMjtDV%2FpVm%2FA%2BJJfev9o7uwzcO6yxU23sc%2F7lkS91esMZwuupqrtJ8dCA9Ww6AM4BP0Z9mkss4ffjCZjF4wrERrhd7H3ozzPGRnE7Os5uqyFZMBzjKQVs8tcOCfqmqxuT9U%2B&X-Amz-Signature=88110339fd09811513b8dbc57fa7695e998aa384c3fc7866349bbf54f50df5b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {    public static void main(String[] args) throws IOException {        //1.读取配置文件        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");        //2.创建SqlSessionFactory工厂        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();        SqlSessionFactory factory = builder.build(in);        //3.使用工厂生产SqlSession对象        SqlSession session = factory.openSession();        //4.使用SqlSession创建Dao接口的代理对象        IUserDao userDao = session.getMapper(IUserDao.class);        //5.使用代理对象执行方法        List<User> users = userDao.findAll();        for (User user : users) {            System.out.println(user);        }        //6.释放资源        session.close();        in.close();    }}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

