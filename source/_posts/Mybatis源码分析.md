---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TNU5XCON%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCp7dEUAAdq6tU4gb00ihItdng8AF3ZFJ3TjWUBQzsOzwIhANZ7MFL%2FswKin6XfeWjMaQ4IChLkcB9Q63WBdG7H8IyVKv8DCFoQABoMNjM3NDIzMTgzODA1IgxJN6k9%2FsmnR8%2BEmywq3APa4Oz5YeDWXAi0EvK0jcZfL1CnOQs%2FRto%2BsvTRswTjtlDpU1sPyuavY9tVP1YzYMEe0lkVCiUjp%2Bc5GiyCOcoHEl%2FioKmKsX%2BLj3YxkAh1ujjDPwshR775K9uZ3rcpT3aeYFe22S5BHkb1mSMPWGYqaQBTKVo%2FHUSsmgb3oSm9iAx5%2Bj2stTINeHSZC5oDC2f7BkmCxGNy12wi6M5joru1UXKHlYXs26ox2hoBcYPvzSWv5i2PWFs3bdZOva3feljl2AoQMQUqMjywH7fwRurRQET%2BYFd3D2vqcwcDfQApS06VjD9sUTJcX2SwchNcy%2BOvfSutYby2ANrtEsTXhnIaaxBttj1Urzb23aemnOT4jOCGGUZkQcyyBD5465ZAmmuqfANHaHRROIo9yOvfqRn88%2FKGDpWn9lOly8StVfKJqnT8ZQK3TLET8MGf3VIWEuHX3ixVymUbFOy0r%2FndhBuDqnuitgDGPCqb%2B%2FkY91xwLppCfiAe3W3L3D2qwObK9P%2FdFfgNzEXhHmbKTQ2T0UJZWOz7XOwPpZZt%2BtlHQoQlN%2BmBVtGaQq5BCmIe44zZC9MrBh6o9dT5ElPk0kFbFO5l3LO%2FCmUNL4WGyl4lHgSK69OTyakYg0fH7ah77zDn8NnIBjqkAak7GpHN7jE%2B2pz9lTRHhNxGy5WFwimXIjRTX%2F45e9KS%2BJunblVjvRqdklsa9pWBGH2p6y5ujk9suDQVHydTjN8jhRGawoYBvh3Nu4PX28yAlejQh3T2F0DAIEUk3JkemXD8g3sdWJmeyp%2BiFN2mcdmC8bPB8cLJaOQj9NDsiin1KM4izGMO%2BKH7nGDDvmAXjWo0xIVWJymRfFOUbJ%2BSts063GxI&X-Amz-Signature=931b7cdd8c35a0c7c98428fd0f61fe7b97b8d27f97789b6c3e20e13180113c39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

