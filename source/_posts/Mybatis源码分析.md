---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZOCLPEK%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T010056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQCxeRPUmbkty5RIZ8qn1pJpvbZPnKV%2FGuhHJ%2FBOLPdSVAIgA30eFYkmJIsK4C1gzojsVQRAUCncsNEJ7oXMXX43r3IqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNGoKYgQM9HLCU6kFyrcA2rYd0GbGlSk5xHPxhwgeMaxtSDT63Y9ken9mLDyyURlha%2Fs40EwXk2WYnBUj1GRsvfd7xp7APKz%2Be1llAZHMXq%2FAU3lbIR%2B6ZWsHkP0tY6diDo3bT0fHu08nUf4Xe0%2FBOLpiCxvmISuIEyursfFhgLaNAeCM4ZUxid4%2BVvlhYjehoE3Pk2L3rJgAz1ZoGvw3U1ZTDfKC7cJQ0kOJAbVAr69n10gkTJUyfvbYkK17bsAQTMVHijzsKCoC%2Fa1IrVrRlnKVBZ2jAYMtKfY%2FpjF%2FazFFprZniQbNSMzacFe%2FhwoOx6Hh%2B0Cxxq15TanvyFj00jeNQ4GZsGLJg42wRtxn5UBKRPh6K68now5x4XD6188zNRuhTt%2BtQiEEYrnxVZtbzrGuJlEEIFnyoF%2Bg0O148uOwi9Pt9ubr%2BzXIcj5cIJBeoZSJVitRKf4NqnmeGHXUkr8ra%2FJ4Tlp98858EN9ip8N%2F1KOdhrs7y3uQ5VYBUquc7k5sP%2BJbO1PqOMrF1Dip%2BIOGg6IREDJY2faRqsNzqk3LVsZFSy1XO9Qy%2FAIsl1SiD%2F2azi5AQU0VSwbUiE8YJr%2Fwn%2BGV91ISBa5Jjx7TYCkUQW8X4gzsCSWACG7pdMi6FVbj0oDNJ1WjHVnMKz41ccGOqUBg6hOJlDTP3CXzAtzQaHud5qp8aVy6cEP3i9yJDfjsNhUzdlDDxuuw0z%2FdOFOwPlL2u5uBWZfHKFPIeJP3XlmeJbIWmXG%2FVYl7Bn2B3BJ5VWc87dcHEYEGsqYVJC7KixEMQCLy84QWA4fKib3%2Fbn4uAOwA2NDt0%2FuvBvo%2FplEAhcNaw5q%2BlW3cpOs5bNB%2FBepkaG1YyYSI4%2FqqC1vIOmz8t4dBFlT&X-Amz-Signature=51ad60abf736f1839c49ba79ba7b6295a56591ba43d886ae3d347fcc5220736f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

