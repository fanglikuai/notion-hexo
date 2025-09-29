---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4AJB4KS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIA0K%2BNffrDUt1n0NJJJUGuUZAUao%2Fhstpo7l4T4eMg4nAiEAyF74rs5sp1XI0etOu6MGjU8IAwxKWhbsRgTXtS%2BwU34qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHABf1DqoApLcg5pSyrcA19kFD2GjvaYKB2PsdP%2Bk9qYiIypSaiE%2BlXH%2FmjarrvFjrGSpnfFy2AAT6HFgn%2Bc4MEgqEEQ9oBW5e3kPLVVAa8HnZ3C%2BOC9s7hlH6PAP%2Fkp0KLsEK0P%2BcsDKUh8s46kAryYso4WpbiqHEIIytQMw47DDD%2B9jACJ09am3RSWCFilyaX1b7fDF6qYcAgqlVCJW%2Fw%2FU5QfqDeq8XfbmmuPrKeSG%2FNTZDqXoAg6gSPNm1%2BwZ3O5MjoCa2Xj%2Bj2E5VbxiIKCP8s90MwY0nqlapFZa0KYtsQgPYbce5273bKUGkxc62LfMz3U2wA9mY2kEPPy5PiplhpsnTjlMy5RkHWa%2FAI%2BWGaGElVWNKcRTa%2F3gPrEMlPvOnQr2j4qp4xWG0LL4N1xkFKfmID4tZIWEFRBwk4jU03wxA6QjLu8E8OQs09UesFzTMn6TjRqampENBd66DsQ0WqRuci%2FBgs8tJaXQW0ZLxyyGD%2BakCySXodbSPEX7eJkfzQTOWceND5F1t4sXoergevStBx2awQzudSd5xWi5tHVXPgmzYeQWPyDOhD8Snu6efkQZwINDETZoJRa5TgkiH3YDBIvdBZLqocpsNsxLXNsjqMXhzPqQzinJcn2r3yNwJ7rUFJbDJh4MJ%2FQ6MYGOqUBv8cnECZvE%2BiLkaTHNY1b3akqlaKRDX7cAAqvkyQc41T6P%2Fkr3S95VpIi%2B2tIyQc8svNUyMHhYe8BcC8M3xlPxI1YAY%2FmTmW3JzEBt2xEpe6Oum99TtQFdtxvSTV7Kr%2F7VBLWAm%2FItDv%2BsBuZi%2F5MHpK%2FGrKzAXmrkXx0SqkjAiUgoEeeFGAQ%2FvM3aBb9YsZSZlRSVgb26%2B8BxswAk2S%2FPvA9Gje1&X-Amz-Signature=e07d5e4ca0b5e7884746e952849c56e24b85d77e0d83ea7305b2e18556fd94ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

