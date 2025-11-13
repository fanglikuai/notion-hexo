---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYAJWVYD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDKiYR7CWFBuHGgCs6%2BsxdG65%2FkyYjqHY8u619o3wWPZAiAPCoBdnXQTw3uPW6gHYU61sl9%2FVASsh4HJvgV4mHPnFSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMu656bkVd3UzwfhffKtwDqVVW5Oz1ueFiC4Tkj4HdbARgLL4D6Fd%2F3%2F1n%2BK6RJYo0m%2FGpy89DZgCHegUk2Q9QhzSNpcJq9xHE%2Bvj7rHx0lkbXMUeJaaNW0Cu%2BgIkNUm7zoD0rc6VRh%2FMKPfJM8CxBfILHoagik18KKij8myn2kgm1ZdvrVY2S5oLBRQXPmX6HORtz3NEWmuOUHGJdz2p9zlgrhfadtddwtzHgSTOHu5ts9k%2FcoXKU%2BfT9un1zDSJdoPGA7%2BWHz%2BiEQS45iUrhnSbnPUfoeLw6yjpQ2tKUichjIknl6xf5Ecvp5vQyqKFMiC9frrql4jC1G74AAnDlm%2Bd1ezmFHmrACur98Zld7agOZ%2BhVf9DM6XajS5DyU5xhNiSteYV%2FDQvNe6qzYkJokT2fRNCH49nCdl1rr16vvOxsQsXC3Gwe%2BrrSVq5kBqnRIdR%2F3FVS4tLU5IsMj8WMGLmZAvH3KZFdy%2BTQaeHxl%2FpR4hK6IO5DEFu0PU1CG1exVr0d86hr7BanMc2oIBpWP%2BEX7jxHL%2FLkXXXz2q1TnFzMHMeZ2pDm%2BK%2B%2BoeJiN4D6hZrmVfSUnzm38Xm8E5VqPWScGlzzMEa61XU%2BH%2FA047XEwaLetMF6W%2FYhgnvb1X7FTpPKxLCKu7KRLiowiPLVyAY6pgGVQEfn744PMy%2FIz%2B9hQXHFiJCUVc31khTM3lmozhw9W2qqQJWW6IXqXYBcJqkERxF7UUzjp3ZA1gyzXml51A5qwBPIhgBYx2IadDByZNqo8jKJjaBqzH9Q2jxU46GRQlSFMV%2FS1cFqF7K5eloT7ReNCCWGUOhoT2n6meks6zRsx0cZDZnnYoRayRc%2FGMHoOcGP6TRQE2paX5fnQXB8jUbHF8EKQXZH&X-Amz-Signature=1422e06377f9e8f0903fd42eeb725c5dad108627c89703e4cb9e58205d1a57fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

