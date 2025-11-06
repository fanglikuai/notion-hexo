---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UN2KFNS2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7D1GQP0FNLwy5Soy6Wg0g2l8PGw2DQ3nAgTtsEtLu6AiA1yoRtWNBP7Nr9HTfgVLoV6bprKsk4Z6KjOi9UXH5dlSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2Pjn3AThvuQVYglzKtwDjVJbaMEThicEwG3H10TXbetT5y1ksjTaTUQQIYobQMWxEM210XBShHxsKCLiDWDHyibI7JVb28Sti4Z6FGHMug2UPFbFXB5z%2Fekp9sXHhgP9WLKAc8dHAhpp9ofaez%2BvG3DH9XAfyYKc0qiSyEry6wal3cqt%2BRUkNusDmvIVOZQKtapyi5TqgjKtIelMFCVWr2wML71R1AJO8Cez7K%2FMZm1GW2s7AVKU5sDDqJK3G5gbPS2ZJD2GfANu5WyULo%2BeKUqejgJZG%2F65VghnCVYsnDNbbKUhF4b9ulp8L3ViFtSsKt7zfh71ykjEmpi%2B3o0bRSSq619veb7Or6UciXOkIwC391Na%2BiUd52kodwU5ixdSC0%2FM8SidgUuK4jJjySbcxq3rKKX3ah83ut%2BXqECQHg1RhYbnWXTXuLbJO8hCVac8nVp5yJZqG86jHGi2QogfjiNaN1tv7BhprcIjUce6UAiGaEQTUUPI%2F6gxvkVEJM%2FVvjuLd5g5ZW5PIbA60AVfvono4IUB24gB42l0lwa7QPjotVovCAZ1XjdAD%2Bhk7sGmLC%2FthwSl5jrHjb0AC1Sr2Q3Fu2ymPJ97E9tX6Ey7T7W2mMxrnUFEYvR7iI8BduoVyhgdDMpQddcPdUIwstavyAY6pgHjhXiJfrBW9jPtem0dpcVJBgJLDiOGW%2FBnJLvyMZMf4rGtsv2y84bIyRIbNTZGpfNcpoik2XbW3TyB8svPJZ3mjrMGFTlYt2KAa7aY%2Bit%2FwPQVReVlLpTr6JelTdXFBCqqyjhuFiEeQCmx4SWVOPZrP8dYDvZPJ53dJbDjKQiLjilMIALzKTn57P1fOw9KFMvfmcgIamT4TY%2B5S9BtjdfiaNEhqvY7&X-Amz-Signature=769cea57f43cab2a320a3740271542a1430fe5883f28e6d33b3e9296f52928d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

