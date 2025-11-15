---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667N3MWM6U%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC8OCvgSU8YErJiLgQZNEe9%2Fd7wRu1r20nvtU9CSYMr%2FAiEA1HQcpoLsqlbMVca92gVEhiL%2BtdakM8TiGNJKsbzNYdYqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsHTw%2BTIwMNBt7kjyrcA121T%2BZE4Yx%2FcwUvA6B6SC15oIm%2FFx4LThMkTraC4NBf5iiAaRk9i59xG7zdQlKVv3YI9CtVZ4Nr6HrY8zszVgshdMCS8Ela1Rxgw7kQcxgaK80ykFugEUZA3LNTxuELxhIlOLIQ1K%2Bdn6UfaG1xKO2YQpUdA0RIpUZl9NUY1yS%2FdATXAto6uLLmcjo7yHJykys2zKKaJvJfcmDReM6fO5RkT%2BbRr6%2F000IaIZ9vJNkX4k0vmTLJOS%2B9fc0dNUNOXcf%2BHfw2OoUg6Xrhttza%2FSBFcK9P8rg0do8KfZSUB3kgA8R58rjWrov%2FTgmeUbNxIBvi1rRhQmNegRAuJwyY1koCqRvISYKtVh0Blk6mMf%2FKflBeDOdXL5BNYZWXAviskHWmqMiqAwvEgS6X%2Bligr5hnx6mQNcTGRCc3NgwjRbmEw%2FwSy9DuELmlRfaTXT1jWdjaZpD7qyWYmwYh4IcH1i7R2NoqhRJjQR1Kc9oGmp%2FrunvC%2BUedOlOPVBL%2BJVO6BF2O3aHY2CDI0qtOAxU2ZrBZ6QoewybQ5GRUmSLPQu5Q2p49RVluwN5LkbDDcVfJmRYYyHLBbRs8XWWLnQ5Lwk71ACw8hZoFjGMSZomTYTqpj83XdgV%2BubhkNWeFMLTE48gGOqUBfUZoHRYOCkSaMUwbczVdgMZlCZNrq%2FoPSRvFRDCG1UCmIGnhf7KThXGxtj1rI9ataA1WMgng1HkMVoEyN%2FszCK6IBnrvomT0igiMTg9pdVH5fjdcdh2Xsb3fslnWllBrIW75gGxTLDVOW3fEKvLDX0RpELuqFpSC%2FmhAnMvvd%2F41arQ4wypzu2UX13R1Zp0no9M1BQkt450fZ62j%2BdJ107OU0CuM&X-Amz-Signature=e224fab84233b84fba5c648776e232d6d10d9d24b775ae8a9c13fc9467b1ed8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

