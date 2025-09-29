---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AOJI7ZG%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQDa9Nn6eBaAJ2QLCf%2FdN9z9J2m3EK7KLvaTm44tsAySdwIgPrilRDvl3OLngSOfw5%2BCQPSJ4qSLQwDTlagsB2tGtuUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPekwvHSJPisrbILIircA8KEl%2FuWZpFdrz%2BVAmIIJeP6%2FRBnVjyrWMckQVDIn1nBTUQIYCxad5Urf1Lg6rQgnk2mEXPAjePNwIm8qo7PIQAgwls5tAuW%2F6mst%2BFJ9qY0HOpe8rRilVvaTu2nQXL9QoyIt2iK9ETIG3nnZ0vXaUAp2cC9tfd6RqOYJsHO8il5ZmMaBsu%2F8u4yckraw30byXAX4wozEm5JjJZp6zeNlPa2RAOAr9hlRIxZ9g33rBXMm0gyBv9rMNeA9sM9Bm3L%2B6NNYs5tJUAHXqE6RAwTPl5WQRvcfWXp8pziprJ9jWZfG9fxXHdlcMAvVp8jTBUQ9iKu3sfrUIVPq9AyHO6sNYvmwLihY%2F1cKgQ2cuDESzi5%2BVvhbED6o5Fp9whH3rItk9%2Bo9dv665BzO%2FUdKUwMtW4yzlqekgEfNVI2CHH31XAXUdu%2FtfF519i%2BFIqafgT3SS%2F92d9U9llB1adP2KHXOTo4oJb2sScnQ57t22s7s3t81bY7HlIQy59GheTI5LIP%2FQtNZeFkoqBn55kCmhC33%2FeH5S4mRIwxgXI%2BQ6wWatF2VRGA%2FGTOnizUNkeRxp58f3ns8rG28Eldu7RFDsdJJUYGaMxZF%2FS%2FwY%2BL%2BuNjYf5VGrd8juX2cJuPuyNXMM7U6sYGOqUBZjTN5DAo%2F4S49nCD2LKjvcW7l1yBtxD%2FN4mtOtj0s%2BgPG8FdsTyWiYReHw%2Be4CywwwOVrItgBPXMrukZw2u1DOvO4BsbPtjoHPB2rx%2BCcPQwe8q9x3Jvici14aKHg76jn3K5X%2BQFG5hDaUNnLpBOXF1IuwGaaNl82OTV4Nq%2Bxvy8L5Bqxo59hQKrMqJjGDBb%2B4tW%2FEYwqNeipuJJ4TtcshFwBlZA&X-Amz-Signature=306950cca03f2f5c584eb27e132b31e466413ec6672dc746885cacb74b1db61d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

