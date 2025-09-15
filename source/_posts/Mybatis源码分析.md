---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNPG66P%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T020003Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjZ33VKvxapVpUM3%2F39%2BEAwzlNdAWkv2Tdkmzc5Xi0RgIhAIljVwU1npn2GHmJOwvDzK9GmHi2J34z26c%2BXi%2Fmp9i%2BKv8DCGgQABoMNjM3NDIzMTgzODA1IgzEFsj%2FJ2Z6giPH1nkq3APMMOU6x8ujvqxyQXo6fcgO%2FUp9XaZcxFRiWJgC%2BaoIxiIaJ7YM8aRUp9hlFMixC0iHn8p1HlpfVqopmQ3KTNlCHLgrbMR6VpE3q0rHKIUDuviNUlTvNwZ1Av8M7f%2BHaKfeokXCzYZLVdFmolrxJFhdvpySrICnMHqGw1CJElanSvd0%2FqMCwKGogc3s1cgkzyCuovxa0Zr3jULKWP9hWmDFP44tErJUt4bB%2FpGl3bEJdNlBdfq5pnPkoo4W5a5V%2F8p9Wlr9B5IWHjpzb4kFFo0jT8429jMTIfXj7lnAXZq0cGLjsM5eSD2BJx2F6LHb5u7K63%2FBeRf7nBtgPrVH%2FE3IKDtYFLXnr3BCZKDqC0pkC36IOwR6Ep4gZH8K1nwWxssZEaCw5pnLFVr23rpIk%2FWumGWP04fc2G9xJABT5myzG4rpoWzJcivEaUKi1PkY8oCkqGFtf5M02NRKZS1I6hBK8sPj%2FHxLKpYSzmzU5PAZWv713orxTwll8QV9x8bgJhDNU8kWIL62%2FyKu57552fe4akbQ3qnHYMy91OQw6yS4xI%2BF8bBxI6kY4LYD7InWma2A%2Ftv4FZdFbat4kZpWE27ApkJ%2BwcBHM8%2FVbsgA5iw5p0CQbbVvj%2FHbRygqaTDsnJ3GBjqkAfgRo1xKZ9%2FSEj1%2B22IA5YjsA5IEO5mohB%2BdzyaQGKAqNTQtElsOyQ1EkNin%2BFrIwdJf8EAYsyIwRU9qV%2FeNR0cLcwktg1MCkg3X1sKSZYPt1DLme5M%2BMTykQYUo8AyAVmuW1DrngQQ7GkKnScowh%2FPGRkLYn7l%2F9m5Nez3O7vDS4E%2B7UpTKYbD6A9wWbiyzEY%2BhR8BhuziNQvlabwzZkmWBGWTr&X-Amz-Signature=200169e9b99beef15f9d97e09d82764ea6c2c1b90cd90e182084a343eeaffb7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

