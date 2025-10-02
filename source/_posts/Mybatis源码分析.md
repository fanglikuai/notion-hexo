---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCXQHK6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpO4EJcgDOOtPIu81raIBT3lRwRuDvd5MLieP%2FS%2FexngIgEZKBAv2XYS5uelzI09w1GQ%2F%2BIJI6t%2FAEeZBK%2FIt9n9cq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDFNmb%2FnYs%2Bpn4d198CrcA7IQZdO2cVdBlSpMjrzKjwrhQHorohfyef02zxsHLV2j4LWILRRZO8i3%2F72uQ731DZ7d%2BHsjuz80ndNc7gBy11v8TJLKpqeRJAQskwiT1LygnxCs2ugWThLzbOgoVMYTUSOlteNSxMUajn2BQbKpKd9QBXd%2Bz53yEvCVtZMuUfJAMcRxYrEl%2F8MwI2EmO9SmonUzv7OwvzIePSFMjOOEYMqgGBvJOILCLXwFSKLeUj%2F%2B%2FWmPayq1K8ErDi%2BTfe9CO3UvKcRZtzNKvMPtjP0xw4Bo6oMDsrDOtuNc9bwtrBv9I5GZlpWGMwv%2BAQzlL8aOfJ18tk0egGm0QHLk%2BEXmcWEXn9iRtK6OGaM8oAIE6wpKiBsHSx56aZmVs6vKIWsToaO78QHaJttx6mKL9w5079oDAYmDzXxsJPkAukf9v9I8nGLgWiEmB8kbJRCmmftEt9eWF5haGKzi9upLczrfEwlg1%2BkNLjViyb3Syjjgil7op1DYDG58YRIxtOa8cLtyqvbAT%2Bpyjo82w4Bb%2BbVTMhHcuOqrH8YR3bb31o2ZNTJs46Dr4uvrrdlc7KA%2FPysvRfvDT%2Bek5Q2P8jYtMRfuufEE1SIZid8nkswltxAsclEPWQGGFxYmKS%2Bbzp%2F8MI7u%2BMYGOqUB7ecPulxFyZtYwy%2BHPlTZ7YzcR%2FOEHYT8MfvkeeN7gpKSiexmx7xu0xVJkJAUwmYR%2FJNMhI7hxY1GZ5BnG7uR5g%2FACZoHSmec9QNJ53Z3KFlQ6QLfg8luhFTC6IYWG%2BxHgVLhvQzKatgz1tESgYvtbdhITM7qDY0fec2lvbazS0cQB1G7%2F1HyuQ6cmA6fdXB3LKFMzZMaDfWzIOMQiFAL3PLyhkC%2F&X-Amz-Signature=0a78cb20cb79ebd00ba3dac3120f8f78f0c15b70d409c3695e00d4ed88f4c08f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

