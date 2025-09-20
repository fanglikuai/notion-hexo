---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MDCQH6J%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCICmPBlh33mEHWE8B82%2FttqIcrHjFajUFUWWl7xTYRghwAiEAnAZwSoi0vbhRgaL4%2BNrGakY%2B0HKUv701Y3huhyNLLKwqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8uRSjcywj6yDPGvircA3ZMHfxVky5CU1QuQGx5Wk6H4jRwSLsPE7FmjOEXjuqzUvKQ0NRtNjXylqwiHRpS2Xfl8eAQZjAVhDserpMMsXdzECss7mXNNhb0qkhxo7hEajYGbL3RpScDjf%2FI7o8PLst5CWdyKwllZ8sJhU1kiqMwVK%2Bq4PPL%2Bk7r%2F3d8YWkhEi4TTq8ezZ6CFi1zKZvH4XAhho2rGmm9e53pNijadQiSfQ%2BdStCqT8ODaQU8FClnfPneZSC89ALSWvnsnQ%2Be2TQKfqoMl0NnOxMP74SFoA0lffkkSnZ4DYa3oWX2JUx0%2FDBvRBbTwq1fAg78iLH5BaWmiUPuCR96n%2Ft0eFBZE3Np3nL1X6EWLdhx0t5W%2B2cF8KufGdTP4oBJ%2BX4vkwu7kGBx8Vam1GJq3%2BlkukClczt7MveGE4bEO6QsHH5symLtwC5y6StbV09imp2zCymkG6BZvlpFVB%2BiIJPcXaTs6EV49Kvqg%2BMjSDfhWs03%2FwR3RBuK%2FUZXRHksGGDhP%2B3A9L6g1tYwCxMUgGqKIPx6BPR8mAjFKVsTtCRx0n9UVZyr83VCXbiJ%2BbQIiFcQImnD%2FSTQZ4vQjl5FGQzD29s0FUL%2BLJEBb79%2BwHwMHIbWmaGAVUOm6nBvqCf2vQHoMJilucYGOqUBPBEkFuhxdzK%2FONDXY%2BdRZMOtoa46ES839XOW6dtR%2Ftld6yTbQIbmZWrSpLpxxRvO2TrUKdKnq6PmSfIkOgvYvPIY55RSJUdARAIjDOWSYiGK7dLf12AtpsBxgS6LVdyReSLSiNxikI5e%2BJnwm8Xa7GpmCT1Oo19SQBxcQNzJ72ckHW%2FOZp4IzmZ6YiHFv8SJXXeiotoW0NqTXVtkr4LXg09DyFYq&X-Amz-Signature=b0de81de059565769d8181ddea566404dbc97704714a2385e310887a8cc5aae4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

