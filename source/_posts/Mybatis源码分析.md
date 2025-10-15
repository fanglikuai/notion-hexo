---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMAODMEV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICKTgkSpcPEUnMal%2FWCLctP2hdxyF0Qi6Swq0zML%2FB5BAiEAqz3jrpXQlAVj%2B%2Bu32Fs6WTsvVt8whQ47fDvsR8r96LUq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDOEpc9PEWtrJJdME5SrcA%2FHJ4QxgW8EXbcVVLJF47NvLcRxeYlBhFrPOjh6DjHIozjW4gQRM8vMkXGvg7Fk3ezDl6nxGOVUGoaVpopap0xS8mG6dbaMll%2B4WbVP%2BXylFwq%2FAbwb6mCLcqya6ZFXN3kpV19hNKUrBsDOuIiY8uOl1VoilOTRyb3l9EP29gHUSYye%2BqGaLSRkjENGaPnOMkl5Lr9LkS5keoaGOn1APs5UKm1LuebCOLmRQlz8ME4xMQ7oNN8xKKqwZVnyk5V7vT8rsyb%2BLJTSFjnb3NBt6AuZnuQwfXmmruM5peU3jx0wT54kNd9QP1e7MIExAXsm9efMfk64zAdfGx%2F7yHkK3TFk0ZfLTKYyYw6ts2%2Bdfo0IrCwBgrxk471zkoZv6we5aTow7BTCdtgqoHHBub6Aj2kP9EfK1uxtMjw4nLVHDg%2FbeSylQFAXVD8TAmjhWUBMGYZ9Ljr4100vPS9I8YQ%2FlIkb2WNixL0cPxr7kvjvMWCmrmh%2B%2F9CBD5qBL1%2BcRcviUly%2FTT0WZqYCyTTJ5wix2w%2BAUXMFJWyGvPtdwnsKvPgV8AOdSRYP4XnJlKF55tqtdtV6wHcoEih%2FT9hLei0YwXxUpiYR%2FHLp2XKIDFkRdhHgPwAxu%2B5UxwuHdszDoMKHRvccGOqUBSXUfY%2BVVrtTuB53nlnF2nnLQPmCMBnqRWMZ%2Bih8wHx3ypsQB5BfgUjlTLN88oU7kHkS56GAk%2BeisZ%2F1jMaRax%2BgtT%2BrswU58yV5THyx8De1X34yGBRzjJWBITIxOTHk5jxE2oG5SF9o24wkWzouDiDmwEb0NajGjkTwwGVbo07vs3csoYuArw28UEPxPggfdIRT14rQg3cnzNbymExcArO0A5GJ%2B&X-Amz-Signature=1c915140efb2cc4e2f794fe6552fdeffa63b58864534e74519e1fbf6273705e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

