---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNXDKPJI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFzHRcAvAIOJZEaBG%2F%2BAu2H0BmKDF19GESDgpaiJyiY6AiEAwZ8cmM7Erz9kp64T1376WkD9y3NgR2q25teL01sRdvIq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDEgMw4WbstlzoYjqTCrcA0dcdcG91BV8p1KgEsEApbiE96wlgheq0b9zjLCZ%2FdaUyffDaQuoihmnzO5Dz%2BRTHvHvkypffqkrW%2BDOTV5sTjmKBZhsqZZblcG4CA%2BjYTcmOBnFY4vJUZRqdU5wA7BYAmL3rNd125yOQ%2BuTGUOL%2FSaA%2Bsebb9RiwisUf32yd9jfUCqn3O%2Fv5BwC6CGGlit9UwJpklURUjQaT9dkGlyqBNv3bLBFXp7Dsmk6%2B3r3ImzNaiJvaZu4ki7DLj3k6xjCzF4EJH1FNunJToiPaInHOQoMdSXKXqx3689a%2BY40xODL8SrQU%2Frmj8rumpupWp%2B25sqwdCzaTx7zU8SVlwmHgZWcdqzQ57m8CeEC1cLpHNfwFfkPkjnUSLHT%2B3Y5acSCA7Y11NrH7uiQygemKjDx%2FMqtVzDOyNb3e4aC8ugM01DQlTCMEpoXC1FEUBmRc3PmSvVyDM5RtIoH2s7f7hWQjP%2BTM4YXopSk6fHoLxxtaMrr0iJrxviyybiQFJ6gQvxRX%2FlBrmqXnpAE%2FVmVISrqpmva8GzB837uZohqMg2Ijaz7GC1BPMMq0Wgki2bwx2wL8BlgjEDIaX1jjfz3HGROjASBGQbnV5%2FaPTKnnao2%2BW0Km%2FBE%2F0%2BkJPmiVUhfMOfsv8cGOqUBP0lp61GNEYyXcYl%2BYR2gw4OvptaqhZ75eAqlRiWFJQvbi10oF2OdOGc5fQekADl%2FAps495OSds3u1ZcjjTFxQFGdxvSCqm93iwoCeBFLBbB4aXf%2FF6UEBo%2FhL%2F9XzwSlsSB%2FCndHV0ZBJVv9y1SuVydOmq0taY6dGAHNSgvQIGQP%2FpLcVecJIY3lmYuIBgDdm9z%2BC53tKcxoiu2JYt5qq%2FrZBUib&X-Amz-Signature=95982b06ca6a07ed16f251aaba241f5ba656db6841214b161fa456977b2128f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

