---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZZI2FBJ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDdN%2BWGpRFAx2i8Ct%2BOC8W33ScvfM3pjzxAEKzWU6kZpwIgMuOpQdVJP8Aizci6h%2BbhLU65sGomv2YjUGoHds%2F7Q04q%2FwMIARAAGgw2Mzc0MjMxODM4MDUiDG3C5R52zRvyzOMQsyrcA%2FF2Q3AMsE87%2FW2o4j3UxGJyFLaXKlm%2FIw5%2BvaqCRgshXjMX1lA37jr3ptfMsIOa0tjX%2FSSyR%2FJ0hKsLZfbI6lpHHvfuezgU1dZm0EpibnK%2FLbwPG6hwwEq9%2BLWvQ%2B8p3oPbmN0AXpsXvW9dSL1KPEwR5N5ag7yNHxIP2fQ26ghbRco9sy67TBpcpLngFDyYsIFnJjtzxj5g3VApTvboRV3W7ogoB367KAR%2B%2B0Y%2BKEZ2P1ksQaaJmj3HMFA%2FTAv8xO%2BK3E%2BtRSsH6o3LQP2BbfmyvnIvksIXJ0TFydJgDWIMT9nznyWRXMOyWHC1U9SIt8DNAl5Bsd83LWgZrETijn85pmFP5bI57Wu4DO8agrh6xou1hJ%2FLlzlfCQlKo8sQvxBJO0tM5qCW2IL6dWz4fcw%2BAZ8vQlFcjeepE%2BHYvyNw2eo1TDNd0e3%2BnoHXumTGbxmaSwMbkB6xGBtV2fQFM8uRJY2G9mQLo1FlSeHvE%2BkcK2nFCyechIN1J3zeBEUa4C%2Beni2Iu0i%2FHlDpSzN2eTykLF%2BshcCDvM916a5Pi0qeZoJEq1CvqgHcQoZQuMVCDUbRc4biZr6xKR8ikQt8p5g%2BM0U%2BjLO%2B3UwMkhksKP2KprljJNDlada7LX%2BsMJy3xsgGOqUBcgx1WAS9YtA8TTj6Q%2FOpz64cUJoorU3FbnRPWz6r0mUkHXO42OELrjjEq0924%2BGEbS%2B7rUPmyAfiDUxyQTAuEwE294APvMQi018gB1VOZUUqNkHYIAm19mDbqxOxxpjzmo70K47S8vVoPayruayFkNLLEotWGmqhhrxecINYPz80wXb7wU5apz40ICBT2jNiH0BTCUk4FlJKbHAYV1f12e0mRNWu&X-Amz-Signature=0f79239319d4d593c188bfc6d4ff570fcd32db873ef01d0062b1e89d4ee5a74e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

