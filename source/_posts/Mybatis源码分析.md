---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RK5O3DC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIQCuzTtM8KAA9xsNK38bN7lWWuLP8S%2FqGTgjFA5Wm8xD2wIgGomdxEGcBujJTzv%2FvtUJZ2Vp042PVMBQFZdK1OLSjmAq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAbXDGi2WuJ6PqW9oCrcA5b%2B1sjec5c6AC9pIA%2BK06ZsCzP1vEjm0oAoHi3nEo5BmEdetUCXr7rjJRqRg0BknK4z0KjAtxWxgSoZ8rBKJP6t9aRPcX2%2BCaMM%2BERxl1YyXKyq0WEGOLMtrbndiKnxIgpRrhbjTmF3yd%2BEzOEx4i0dwAATcxyaLISrVtLh%2FUh8DoXgEvRcKatRF3cqcNljoyD0Iqss6AH3QUzVy57nTETlnw0tQzDPEP78%2FXImKVNSVpf3ETJtfiDZit%2FXfFQUoYseuhFLZu1MkUcnwhLyAeAFA4t%2B6JxwBOR93CptjqtCU2aIz8tvi%2FmqNlJiNq%2F1rsomq%2F3QxbnIxaBeZ0gG0oAg6dBcryVraGb1UNxDBXbrNqeDouqNMgmDnavJJb8D5dhiExGvN%2BuaaK%2FPglOpPbiFnToKHUmIsnvR%2FM0DeaGcA9u7Wt%2F%2BY0DCSXgxj%2BRurDfTci4zyO%2BPqLDF97pX4ZYXiOUGYtveHnGXNNrWvYcCVRl%2F88%2FbLNb1DZmtse%2F4IwmhIqN1J4Q6A2NbmuIbQ3%2FN80BCUzlAlsxJ6dUx38v2cE9Xrcz9KNDS7HyFVRUzwEbIlJQ9PipHvTb%2B6ao4h4NojmLcXAxkQx9MAxLWdPAW1O0h7S1v1aXLuTOsMJi6jMkGOqUBcUNJehaFKbZAZ6Gmn7bfP8NzCx%2FsZ8IAGTzmzTGrwNbjzVCMvU6qp41co4Ay8yVSck3E0DAkHYKlshkFvH5cbGhiF%2BokZN5HT%2FVTwo%2F%2FUjw1OJV6JkoK7x1NQdriWyhoU3QOxSUM0U3rdZbI12DA2S5B7wlljtOB5tzqkmfpai4LXW3MF8jh0viMDQwlB7IcRcG4mHEDEOSpEXVwjj2VFXTUoOB3&X-Amz-Signature=1efd34df443f7ab55c0d0c3b0adc43e19b812ca08e8935b4b9559c609d22dd8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

