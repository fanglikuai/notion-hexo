---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MEB2Q5H%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJIMEYCIQDFTJyJSN7wliUT2fUqNL7zYrV5IyNs5AzZ8hJjiNL1hwIhAMffrRdvitLmjQp869Mnmtxs6KyxnqijeafO000bS45nKv8DCBcQABoMNjM3NDIzMTgzODA1Igz4B%2B1%2BLnzJPYhqNKYq3ANmcBFAv%2FpGSjWS0M%2F5%2Bo8qPZrJOexq9MOtL0zD9G5MMDR9FopHscFgTyYogeH%2Bpo7Mq5P9CUe0P6K%2Fqf%2BCf8gxYREWVJqppo8LbV4I3hYwhDZKRqVg00KS7swyOkhHR34nmx2dOhjPP8tCRWEerxAQNsGkB%2BeP9%2FliMrSl8M3%2FdM8nVVH1K7M1kjor%2BRWP5qbU%2Fk0LhDwXNoeAZ7zOoTe%2Bni66PVLgbRFTXbp65u83YmZFemR7qFzxjg8saB%2BBMtizguYdfP%2FBIml2d2Du8lf7tPJSUAnowOXfj8Jlmdqge2uoUw01DcPXoU561ECCGEOVKoRo7IRUgfqh4pvDgDopDYQitgIfk8jTP2L8G6HyDrZzoQSEQhJUlp847lmMRfIzpUIDitTwohFHtg0IjgMgQnM9j%2BWqu1t78ZFq35No4kgRt9e9TUWtXgg7C96nXNdNIm6U2NMd0ztuxYKZpg4jfR48VHQsGo9Z77lgg1tqoAY6BRduRgzRM865xcxh7eK6s3zpdYquBLhTJkg1Rf6bQ1cMsmXHEQuUGExiQemkDezvgxvb4s9x0bV%2BDgDWbvnvC8UXb5kj0RzuzCyivTkDWFL6wamEZz8IGtjDzPxlZwRYtL%2FnnV%2FUYFdEVTDBpcvIBjqkAeN0be19ZCLK4POSMC1q7tBLK5oU191CF8EvyNcBmPgzkbhRSB6d%2BCJnrcuvh7cqtCgx2%2FmB71EYlCc1dHOxe%2BoACHUqA6GhhzzGpykuNtuneAXS2wIBhUOoDHKmn8pw5PZ1HTHtu3vNYjuTHGQ%2Bh7YYNsCqH6h0%2BOZFeMUm1EWfxYInVUDlgc1aGFCA5T%2FI4EoWu4A56OyXBWokpYAqza6P90%2F%2F&X-Amz-Signature=008399282088437af95d18f2cf9c7ff65a60c5804d153861a7d1187b1131a702&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

