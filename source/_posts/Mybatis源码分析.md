---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JBWM7YR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCPi%2FAlz25wBOmqyBl79RubGb%2FouSZ3xq43eS5bNtvPKgIgA%2BdXgB90dQmNTgBDpODjZ4jV0izaU4VEcoMkbJx1m9Mq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDHPag4LjLxE2XthT9yrcA02Z6SoEplcbFzrdhYRuAKqn6MoqkO1t1QGWNEjj97DUtJhUMhQ4OU%2Bi8whQ8k65PyP%2FvalPLUpYXnbDf5ndNup3FG57i%2FkOwUxHhVow9bQVep7uA6%2FwBdlk047AF4hgQ5GuZ4iZ7JMu74W8Co1wxLBemwYFTqanc1ABtAuLfYgM0E8KTM5Ujc7U30RfQQMYiU7GYwk8zOkA%2BvLDHGTgVH5t7rp60HK1qUTu9ZT%2F0WEtFf3ijR1Ra8VlJws8fevr4GurUNhO%2BKYxp5cZsxrjhF6s%2BB0DMXaI8YYTB34JzIy4J0Wi2ghfw0RGwVBIAYeW0JDML8gFY43%2Fb9x2RCL5%2Bc3iP6bVg0c7AiiWtxC51E9I64c%2FHcyfN%2FAF%2BUvuVjsgZg1kjtU%2F5jp1ChbKvKG151ngmctGxpdKt9fBpOzcFsSkW83Ybppxyqt44vlKhvtXDM0vyn2Aleb5UhqBecnNmjcq1s5FUpy4mdOA%2Fck6YzwOFX4Jww9beRur62%2BmIx92RTq4qa%2Bhxoh%2Fp%2BwyEnnvt1sUJ2geumKBd%2Bibk%2FspOes%2F6A6QImC%2BXbezE0UFFFRKLQu98RZlffWIKUTvtZdMrgrEjm2jGsrcfWAxT0Q2YV8OtZ%2BwibQ0y64gjv9MMMna%2B8YGOqUBwtEs%2BCRWb524SxRWZVIesqqut0evZROAfxSlZH%2Bkfugw9763A1N43AKCgPGgKakZ10L2cKWGocZTPaejbQQ2xVoWGKUd2mc3Ynspd7n1Rk048JDUkWmyyhQBHsLSSx47GrlGwdWRFT2yF8AIVbwPJsfwp%2F4zGxwSwlOSy6lSHQIxt7cc3Knm%2BSjxzjyGE0thXU%2BtOK4%2BeXOewUl7YgN9IUhVr5AN&X-Amz-Signature=460e0e8fa4b80c9339732ef522bfbf9e31e2ca0e7bb959087614f1d59c1f7997&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

