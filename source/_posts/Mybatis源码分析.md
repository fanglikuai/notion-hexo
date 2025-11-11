---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQSCY6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQDzo6LEJpWfBvtnc2V%2Bw5NtC2qyeE%2FIc%2Fs3PXZ7UAKjUgIgLJ6S65dAFCvZIx3U6bVPla0%2FM%2B9XkfriIwMpQmJbZ8Aq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDErUmEyAXJDzYnuGryrcAxoSsnv9yZACjcFtKbBM1jPzzdQ33NSU2U%2BBzJYIMlem7zM2GhSR3BGgDW81aYz4evzsfCPDOnRq1L72YEjmP7w6J0e98kGGVJMRR%2FfMYi0UoMn0QtI2Fabj3UL4vF6%2B6AgiWI%2FKDzLrqb2XR91YmqGRAApU9TlgrHa8LUWgDPwjHhXa23Yeku3km3UAJzlHEHIDDlw8zsBjs7KnriQTFv8Ix73f6D7kW1AAlCPcvXQYNnn7LCYb0uW4GaBYxq71Cq1IYHqI15Etd5AyyHwEZ0U%2Fou4qlyuuowxmY%2Fz31XvK%2FbzuH8%2F5M%2BmAZzVgTg1s%2FJmRJ%2BP9Nx2%2BM7BKqOZftBl0aOL2y2AXqVeRWIzhhlUD58ncy0uzNKL44a52LMZgNIUoLTgNbfVD5SKHkB6Fg1dvdtj5eq7NhXQ8SW7pjH9UKbsmO8thP1r1fqme%2B271cTSKL2eOd9MUZv5B5MhZ1eZdtfQCeMQIFKrfvmldg9dIWPNXgIee1JiA%2B2n5ilp09Yr20HgbR3Z3eNICNqC9DjdWIww%2FWo6hkOp68qlizJDwfyJPLRWJfelXk2mP9VKfhAEcLMWjPh50ECPl2rOD2INMUsY2W76bX3KRu4UAVL2ceQIQ7v%2FAlnE%2FIDv6MLCEy8gGOqUBaxK8AUpkmzPN0MMlq8gCrvVOThVx%2BCLLKatJtK2jgMqaWjEf6Zg1Wx%2FWqA4tpBsD5C1fNk8Ft%2Bf%2BxhmoBNI%2FJXml4mDv9%2Fmc0SwjnUrFjhImg%2BupWr2oZrbsoeXwGbDuNjnUZOHis8chPH8UuYcjDZ33aC997OAyp3xFourzEDZGJxmg74mpGMzfl%2BM3omZ80uqMTRImNZ05Mg8jSVxGJeDz1Rl9&X-Amz-Signature=410e17f1ebf7ab0b6cd1d388e9fac22cc6bd9edfdd8173d9041269434acb4837&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

