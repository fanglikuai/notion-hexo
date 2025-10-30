---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EG2RERP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCICvllAKGRtT%2F%2BUmWIpIgoohKfTKhH2M23WwgDrb1jcJRAiEA18Lo0y6As9u96cqpXEV7JTx9HOYab3vwqMQvrma0n6kqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOnFN7IQ4Hu15XGuircA%2FRcw5wkqepkZFY4U6pyvobCd4ClLSv2Jhl3igQpNfXKMkCiq6yzrdKIy7J4sufYoznKmoRyMJj0dJ8jobAMoOR2m1sNAvyERVGbwGfm5FQqGI%2FxlLNxzc6FAvvgm7SJj5tiQDXjLIWkZu8qOHs6AkApW5X2hWrzmlBhzEl07DSMTVWXtd1EnF3cfvYfa9DTOQ8TLJ3JAQMaBBA43TWCu8DNgkfxf9eLaBa7joPKHzkvvWYRC8VW5pIvSiu6cyE%2F%2FGB%2BuozAFpps9gNhXIihnPouK1cgBZ4JxrkrE43En5AoliayQgjuudZA4Tp0NbFezFUgcgmnv7lhWOdx3bdcEqpgcHe5kyT%2F2ShNZkpWDpJHlxaPDtA9vgL1%2FL4fm%2F3e1y4ycoQmvnzrleaxPn7S8m6jwySbCvCI%2BnxHRYSyNcwN%2B4sHskj4R%2BrWqvJXfwAkfT5LpF4orlcKYUDyocIyDMZWfTb5kZ16zoJ41C9oCRmyUguDxqSV2Y%2FnCjzqQvxhzQ5F5SAbgCLjyUSRREf0nMqywi6KlLBGhVLZWSgOzcGz%2BL9YA9P6LeXzfMm9cuKw1950Ju8RoAGgNKKkRxs2FVVOhVdAeJBJeoac0%2FUFR%2B9ZL7Vn5wlRwmBxNQZVMI6djsgGOqUBBr1hUdW3RpttE9oytJhgN48D83UG%2Ff7QFziafkRzQ2a%2FE0x7HVMh5MjJVnhHRBt58QT6GqwmCoX%2FLRHxVQIADbkwXnwWyvp%2F7ze6qJ7EE6Vl%2BbyIuBKM8sSGLzgisghBy6YuJNZSfz3CMrnjk2DY8OmyE08xCxx4gClyAvenLF1epglfxGdejC2kBJ1uEmBvs9I%2BVAEjWyjcrkgqyIQqCBCRm7Cz&X-Amz-Signature=5aa156944a4ff0e386e4ea745a56c51962808b70832855f915cb666da0670edd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

