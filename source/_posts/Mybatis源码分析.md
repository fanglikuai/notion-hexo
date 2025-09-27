---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LQHG2MT%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIB8CWHCGCI69Q%2Bzz0WYAQXHtKaL6yFyZsJCEf%2F8GGemKAiEA%2Bb2booxqFsq%2BERexx0kLEeMI2hoH3IE7wndF9e9uWfUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBwCp2hRnCkI8AEhjCrcA87R6WYyxAKPs6kAAgwndSLq0%2BToP4WQggU5%2BR3wygAFiNoZ24TCHMW1xaChOzkrsil5l4565FNlzdpwKSNQYOxJGE1xd7NLEW7h%2Fb3BdJZp18Eu4y2QTeHNLKMch%2BXx%2FTcoELda3%2FMbu3c%2FvBgw44Ic%2BesczllJ4yXUaGAy97%2F9QOQWO7OMtxJbamrdmaE%2Bi0GoVWBi%2BSSslnNESDCXbdW5pqOBu4TnUWUUiAFdxn41tucuzYuvTNbBUegyT7k4rq9tIk2xHYEiijK0RC3uhqf8mDn%2F%2Fin6nZssM4Zrsj3yUSYokevRBLr0Lrx0cg24Q7IpaL7uzm6mZCdW5YftVXv6uZoWgm5a0Mb8cHwVuMhvTuUWSaWDtoc3NrS5XVzsZJtf3A3WUaP3re%2F53o86AOzQXEfIgli7Ujk91PiQSn07Vpkzyrum6a%2BhphYDvLX0FltVfmDTVwgdLiY5gmp7HW4CxYQ9LM9ev8oFpSFXbeUQl4rAArhRWOtMX9H3xbDR4sGhf4jsAft3ap7T24KlJGRkDTHk4uxIjEoiJaVQdQMu4OOIrr5%2FefGdrsInAUj2B6QwjgimY8C94z7YIzXSWNUDc1k%2BMwURgdSX7nRHrW36hRsiDhuZKXtoZlmVMIPj3sYGOqUBSBg46bNE2TeJOLLA%2B6V8qUe9p7a8YBacjkt3d5u439dfSBAVJN8GDoNHF%2FTcP2XXXKV6NKkkO8G%2FHAf7Un58hY2tvPNFiiEuEy8UgEhqCA%2BiS69Sw2eHwNj%2BDl5kX3A6KGvasDQG%2Bs80MYa03NuhgQZk6uBGySgbt5QP0W187K6yW4JxmEJqt5bGwRAyUzDziLXiE5ASW8heUtIyf0la7lk%2FjEKh&X-Amz-Signature=f7fd0bfc5468e1a4f3b7298a4fcb1c92effe1c74869872ffdb6c3983997f1bca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

