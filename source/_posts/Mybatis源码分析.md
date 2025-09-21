---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TTLF4CJO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBQqfDnFKkxbaAAEa%2F9gqn8qNn2zSc785WoCNayzGLJKAiB2iDnL%2F88y7aOSSEZL6DKrx65j%2Bvgt8fAHvWBw5i0pxir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMp1la1xE4HOuETwNhKtwDprSmcqPNsxhRjyT%2BhCNjc39ZGZAlayYsSOYEuJTqPuFR1pZbqa8tnovzH5IPzUsWKN%2BhkUQFJFunh62nQrT0jUH%2FN%2FyET1dmVscAfEqhJ9VGyrnoR%2FH0GQvqR%2B3DD%2FGzO0oBJI6ZwxiDQtXMTwqpFNcNLw0sXIRvM5i1zkaId%2FOwVxMdtQQi5Um0mkVTnfIt62qMuKZZUMEg%2FGopjfax9cBRo%2FDj%2FTqGzvTQrr2tW5WSjKuKsp1J67TQWOdSYqYYGFEcuqK2pImq30%2BRJV0IRnDsTPW%2FPKnydCTuUJBCXDdU7gSFrLoSJ%2BRfJeieMI8CEvkYfox227uGdyXmyZ8TdWGOMeMsHdMFBOKNIQeTrcdI%2FtR0vZXP03Xn7aWTDAj%2B0SqhIe%2F6ibsVQrttqnDviJ8mhznWwKQXkKs%2FT8QXhDzHu11fV3utjBe0fIxGOFAWy6IJlPwp%2F6crDngIUdFj1dNVle6FROaTD3T%2BGGUXvxHdYMnE78OgZxSFvPb9acj6K7wtiu8ElH0ax7%2FtXTjMGH2XJhpt8Lyt1JG3kLd6b%2BPLgPoRUMBmnKchSqPAtlu8QCCGO1FMN%2Bt0LqupMHVt4GxveavdfazERm%2FzU2JlhSd6ZG%2BIWZAjcfsYSsgwvOjAxgY6pgFLiQA%2FXl%2BWZrwASni8ppc2I2JXDau9iD3ejiAViZPcl8PVazl%2FexAqXDh%2ByRCD3h1MsSx0Kn4NT90VDBXH8%2BV5zXD5rCkPF45b8J80OEN%2B1%2FzjMPE9ZnJfuKQbZPA7CoyxnuFMVaJVEB00tt%2BlEQw%2BMweX%2BBfuilbZVWtMezshbMMo29OzNO9jAjXKSzdPnxQGz5NVmQ%2BaT44CbDMUigZJ3HFRE2X7&X-Amz-Signature=0d1451fd41fe50305eca5dce5de55dbdcc6824d30a418d985c7c074747d09c6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

