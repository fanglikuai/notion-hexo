---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYQO4FBC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIGE%2FrWg5reaNJ3zEZVbsJf%2BcKUqsYfJpiBW3rj5qZgmsAiEAsUOOTtly78DoM3a1d9UbV1o9Q4ZtoAjEQP1tQNmirqIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbeU991zjo2h2jFySrcAyM5cOBTwkHNZ%2BZ%2F9LOfPYxegOVvZGIJYF7mH9A8QYjeTRUSl5P%2F4v0sCb715oO9jn9SH3yyD4FIJVa4ir78RXzBSoeiuEUD01xk623haovRx0Fe48KsNvJR0k1vhSkxdb65yQBs17mecvcnm4C74lIEJedg3X0Mq5Ox6PZhnlKCxyx%2BmBpjc76nhR1Hyy%2FWznb3k3%2BRTLwAFFi%2FN99wn%2FOgQ9D7ugfOdfH8umQ7t1K2OQXbXX2x5eMZQsyQv7QOjq1pNSU2IyyXo6H03srkDh6uzW6ciqAvaT0tQth6dSna4tT1mjPvPxhpHYvM%2BvqaC59koyz4GrQATIeTWJMHfVJU5mkxu4TiE9EnCQh4Qci7rkJnKxWcZMLTpPPSgqXtbIZGISVUfxdJGUirGTPR5iEEHcJWTQ5IWbI%2BAdEWQLB4uWd1ggbviTmDI%2FATc%2FIXbSGGq50D%2BTYQahMf%2FBRjluzJOLf7YcHFvFSd2ZcqasmDVCcvWoPYelVWFiwhQGuC4Y6pf016ve%2BLF0HiLFoOAdGG0cUQAxFmLsUjHqVJ%2B9VPrLlp6ZWbZSj9HCZH8W%2B6e4zDKJpNCGV6Sxe5udN7Mork%2FlscE%2BuKknyT9A%2BX0J5Z1wwOIiN19R5t4rU1MNyIzscGOqUBRcz%2FJvFXJ4iyIQX0T%2F%2Bc%2BTuI%2BQjEKO5DddotKL%2BxPwxfrSSwip%2BSUNm75Z55XOQ1XA5rrejcFVPp6SIUE%2BFr7dMl7drGNJoUOR5AdUqjg3E2TVDc4VGV8kzR14ssMpRC0n0zOBPLWuxBm5pFeprIIaut1IUJw6LkubIPXB%2FiZXgo%2B3C7em7psbfVJhtCvP1OPsq1DDXBStEKXH8wjzM9p%2F9TiAma&X-Amz-Signature=b76c90b7c71d9b468f81a654fd11eb57a071007d7778452144b3bd139730770d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

