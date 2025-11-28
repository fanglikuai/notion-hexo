---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2AWMEAW%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAmDtTT07cXlqwoarn2p%2B5A3fXa%2FMQQl6QBtfvshzJlzAiEAgDICvF2HG1FzQbrqIQ5kEUvkFKWZEXq8BbNKTMhpkCUqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBmVNt8OR7Bp4SBYNircA5sgsxX%2BOkzwFNt5mhh9RC1hVbCEIdd1v5VR999K1YXw2%2BZluo3lbDpRHpz6Aj7ctL85%2F3UwsD8NvaxnAld1EEuLzyzpisp7J2BcvZYCMmjozljpsdDNQZ6VV6a1%2Fl8qdyFFoi2jy%2BU3%2F5scMN%2Fo1v9fvYOhyD1PU4UlYapi0N8L8t%2BUYR9W61Idp0b9YK9zVDGpkVdr%2BkghvKLMAi8h4mlPttdatSqf1iTc0ALnFwnvCpdFk6PXNTbGzPpyXuOZiLN5r%2Byi%2BEzGTfF7kKtfR23cyYQ8cfa2xbMgwd3yJaAZ%2FmMKHqj5k8m12hGj4DkE0bB6UJZOk9H0O3fLPrfcICectAV%2FGxOg1GkIb5n16jXK8GPe04FJeFERiN7rfpOWfcK6YJ1OeQ5CzO9m3do8xQVo5qua83OChVWjqoDYybJgAI%2B1l2MrB7ICGeQYkp52cJ9MkLZHHwkW40GaPaBpQ0DmMvU37ez%2F4PSoNqCkdQM%2BVAD7nyDBg9QiImrLzExAT6tYtOMHGHpQ1DPIloUFKBzTNPGPmwWbTGEvblQ8ud8YdUGmgtYtgfCw%2FoCwRXn7hBuEjih9X77M%2Fieaf6yGMkhZl2TCx4naBI9SwMZ%2BmwDVkWaT2JZgnrXjE2VvMMO7pMkGOqUBop7mRWU9q6z7H81T15LIyNWfv%2BLj%2FtNBEsFadPpQhCqR1qQUXokGMIrPgIilC%2F5xswsgdsHSsKldZ3R0rCNwnrr7T8t3BzCciKpa5UGqca32b4SEfqV7DYcRVfkHMfWoJVRyhi7PeI70jrx0UoAadpn5%2BpUZmKnpp6J6E0Ky6D5%2F0TWRinxXnKqoGJt9DEVmkEDWDS6rvRZU0wIojwG4UIaV1rK1&X-Amz-Signature=662e858e2082ebb53627bf1343f00868f418e7a9de14cfe0c05d1b1a2e78c7ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

