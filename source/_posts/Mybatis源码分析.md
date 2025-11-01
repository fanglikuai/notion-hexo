---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN6YZVUA%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCIGSv2p9ISY9DT2%2BnWdNX68O0Q4LvhTz63xECUWvF8GbxAiBSEky4TE1PuYwc%2FmU8SaZw1UZL2kQnIBBxu4Ft7ttvASr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIM8VEQZeScQ6SIisXfKtwDCaSqLd4XppS3K51IOj9%2F2lA6cECafQ3hk2%2Fr%2FcMy74dLElLnXhT82uGZ128ivzHnRZ5B97%2BdsWk0yN9PdlrHblIAMPwvbriMdQ4zXCS4TKu5jtuo9WHNhymnypPTbSk0IbTjPXl3qb1B%2BYki5rHBs639pqFybks85J8IWw%2FDKihZd2%2F89CWk2bzaa5mYXiajhZ%2B61FCTg23LukZrJPBYGsJ3jvUQXES4Wtbet7uJSSw0Dh9yjj7FmvferZR7NDr82zAoparzrUKJW5JPu2RmHBA1vlD6YS9QKThW%2FsiqYPuQ63P15r4djMcZwt1LhCfkA%2FCgQY4uuPhW5U79M%2F08C34UOonI%2F8xl6UY5kApWwYpFLJpAG7DaCxmuplbY0YmbI9t14IWrIeB1CKVMhaRn7lUxhWSjOItNxEcJyErUCT9GmD8OInlDIE5Ci1q5rBun7uKW59NsfagvBzbBL0BZWs32V5xQ5FeMRk5Ik%2BsRsjKfOZ3BwsXDggNhQFRGQEprwcM38PsaXGffJFd%2B84V4rNXCT7jq1bRZIGqKDGOOfX5%2Fr%2FGXwZPpU9PDlOtzo3%2Fjo%2BYVRZGx%2FaSI97Lv%2Bom%2BUC9cUlJqJ7lHK6GUtHp3feweFRczMJfFU1lnVE4w88%2BWyAY6pgHiMy6msfECk4v1VZby2JvzY56JtdXCn4H6aizhwc62sE7CwK9Z7lRYVwYpt%2Fpl96VNGTYuS1H2pYCw2xfywE2P0wMhnSnkEJgv4iv7lbSqqgzy1LiiR7Z69jvNaJeSYcHKAdLsaAVO8dAlpXNPDFGwiTSI4YtSOCNq4pCZj09jICWWbVeBn98iuC%2Bsgww6AhP9oSswWICcoMfv%2Ffu7Es6ZIaT%2FhGRW&X-Amz-Signature=5c84731553710122855c9c88dfd32ac9b41b7dd148121ca70826301e431b704f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

