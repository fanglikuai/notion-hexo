---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YHNERIE%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIAGhXTWR2z2yfUc1XUocfxFpjnJMTmEjCPr0w4R50XpNAiEAtbSbElgpuvi1BC2Lu9dkXRTZ9%2FWRjsVjnW%2BfN%2BwAMjwqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCPmGMVlAmB9Ex5k7ircA6XWoj8pHj2rcikBBlZ%2Bw4f8xFtXlRj27guNHEg873BUj0UoxQhZSLF%2FM59B68dB9SKqMVDgpNpR3s5RQtj0X0%2FuGCQH%2B6WJzX%2FyNZwo2sMjn%2BGaBTa%2FABRqjwhTJBxQr3UAioGc4HvnZ1TTxLrGYxYS7YDPinzHirp4sgKlnFzMv97WNcCY63CfA6QZRERSkxyQIlrrUhbjHRHXhQ%2FZlfGs72qVrZORAPUSfnOLjDOyqrtGfiQyIol9lAs1WOVugQxV1P9zCctMq8Lqs34XToUbhMGpB5UrPMaz3zl4IyCxqEG64MkM0fgEQdy3KDx9v5zu34Pz8OgAYdfr6Q3%2B3Lz5EBHGiiWVnGwHtAKxS1QJUfUGdX4ieWxFNhNRhULVrAyzpkQ5VYG5SIoWAJDSdCiriNXHBWgJsvRnQNXLf9eZWhBcGoLCi8WuXdFOiQjIPhSWJQiEjKtBWfbcp65hRPV4caTKAlBHfynM5E77ARmUXeJGxkgvF8sZL6vmwzeOv9PATsG24NDFaFHF43x397J814FPybvZx%2Bb9%2FYAVrO39b5GWcfqP3dWhEG9GXD79zKoUdTEjnyLnpYd47GmmpMqk1uYjqMgixXJk3eARwAP9ofLkTPCycVR4%2BLWSMK2nv8gGOqUB%2Fz0k5rx2OxmjKVO9SKFa0s1fcqI4HalflS6VRNewOViuCoOpfUxo2%2FPTxWGbWWiBBbW3%2F0nFCDJoxXHUp6l%2BpEfpSU19SNnCzesyZ3GctjSZ8qLcaZfaZ%2FY%2BBe31jB1jcIeHfUHXcyk6UgvKHFHmD2rmbxAnK8r%2F2rgy6OZ0Z5nmFE15kS2YndwhOXCkxHwBxkj%2FdGJ5xUuVC4MrwDuenyPdM%2BCb&X-Amz-Signature=8fa9a9965a471aebe794b0561f3aaa2764fbdf9788d29c7d890b9fd5e681ff65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

