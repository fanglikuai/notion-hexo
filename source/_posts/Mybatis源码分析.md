---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667I64WFPZ%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIERG89VlIm%2FbUgFHj3trPdngItM821Q%2Bhn1mE%2FmOXEW7AiEA%2BtRIph3iAIFs9cwQf2w3w%2BpHJ5Do7gxKK%2Bk5aV34cIwq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPesoEnMtWc1RdawXyrcA3O26%2FCOqbET4qEf52aFkydXrIsN2Nykbz%2BR%2BIOcNMBi2TNZzcl0o1irp0GECUnN7zt1GwGm3V4kWAh9sKZT5XO5ep3xCeOPVZGIFNAGomSCI8BYp2iPP0Umjwj47u0JhMkBYM1P5QD4XDQdbdTofc3OnRAH%2BJpVF4eVy6cvzZzLLR5gRRjF01O5Rw%2FU%2BP0UtTilJ0oILYRMMwT15ngh8YYm6nLga%2BpM44IA3BYLrG9pCbCelN2Ro1ZqZ6OkhtA07ghqX5TRAEg8A4G5UwKqiZUnMU%2Bj7B8eocjikyp17kbV5SJhndcBzxuQddZRKWRzJ0VlPwcM2NAv0UUSSTVvnCXi4QNiobuRtzMxGTn7OV%2FM1dcaBe0YaYmEYQnIpGVvwU6WVWU8f6BsuEQXslPlk%2FfmchL1%2BTOPXSBz%2B%2BsaDU%2BYA2iWJ3O1f9dCd5siwZ6LkDavjqYgDqNv%2FFj9ur%2BImzKN5WuZmV3OMpWvN3lQSQSxUyTsiM93rV1J7Bh%2BCezNueAI%2BADbOFeTjk%2Bbfp2jna01%2BbaV1%2BFn9yc8FQRWjB%2F4xNcTibwxyeV3xGVM2nqpfADGyP6OihhcLbA51xoQUteoMJxlebKHv6hBzJ7XrKIXK9DoQ721fz69hljhMJq35McGOqUBjyYnkJukGB26JnnhenvkUj%2B8zNb4538tRBQl5fFoPgTzz0RfkXUChAFIZmRFAJVYrzCQ6jIFRpLb%2FxlgdaDnwgHm%2Bp29mezgMhohLm8uT2%2BOX5HQ4IZJwp%2FZof4ujOnb4uTk2sE%2FVJIWFO%2FEhlKcaN84W1SE6dEV5iXfHmznRXTrDhzGZtDhkMfrxCVT9pyqWlMWt1Uq9sJoG%2FG8DHoj5S5OVouO&X-Amz-Signature=f055bfd1685736badb1e55668ed813ebe9ff38cc3e95a742756d97d3a6c33a0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

