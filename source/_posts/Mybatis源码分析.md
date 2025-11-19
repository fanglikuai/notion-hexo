---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI5VXCNC%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQCgN7%2FQjDJLRC8N3OjtWwI%2F8FYqiCfJoJCAOYiIXaQlSQIgH6Hr3isyak2KBo89lIl8dFjTPvux0%2BPzgBFr2t94pkoqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYcOLBVYj9Gf2yn9ircA2dZm90UrS5wIUx5KAjZ9pU%2BeDllQA3XA%2BUpRq7v8XdpZBJgd7fFYR0PBeiW1CYdRmBQjo4ik%2FJw5zz0%2BGNhCHcBC2D%2FfV2Oce%2BMxFL%2BJnFW8pOHHYzCj7Cw3nR9DPu7mCW6uCahvS7i4oknzRDVFlIn8J88A5nbqS94LW2A7k52jkpBgjdoEjhYizTNOU5LBxsp4Eg3QoOqEYC67kY4Sz4l69hki4DJro5IaWo%2F8PNVQCqiPllmqkhugt90zCzQAoki%2B8xbJNAHouTWn3gO8E8YbAzUbkVTBICCmSOFazz33C985Wpm9v%2BEGdzW%2BbMFAutf5y3dcLzAdJ%2FT3MFFEqHlHwQOjX9B%2F6h%2FS88blvMkrEu8gr2zgLcN4BYsu%2FVnIa1eH0IDjyigAcPanpNqDS1m5%2FNCuTYrULTKBUQsHjQlKD4XFaRgV%2Bo%2B%2FDB3NTtCXdR9BTFUQTSnH5c30Hn3GZT80m8gMzznVzApXNoFqPDuD5cNNCSQSLRHpYnv1GFXQxFqO%2FEEFByWRPJfGp1Ma2jRYkoo1jINXR957P78wqr24TW6UEHXGUnAFw1Ia5alo218g54UD3ykGtpPd1EeOD9dx%2B6wDwGEM%2FKP5MsJJrWECKrEu%2BHGqEjwcZaHMO%2F5%2BMgGOqUBT2gPTGHDZfukkuOd2WzzUC0todX8hCJClVFly%2BRFxHC%2BNTFymQT3BelahBKI4BWowqWSJab0ths%2Fj68T48F9xC7xPuxhj%2Bqx%2BFMGvSLQAN1ponUPSvpjUhKJfSt7UEmNhOjT7xJu6MIm5exSH2Kkac8gnLRYIXpUDE3%2BaxnjkxtlllzdS97CDUNovQbmdMpm8%2B%2B1cy6Wh5NUNJuvMYxibayJz5BU&X-Amz-Signature=4e8f341f004a6af5d899bb43a457a6731c45cd47bad4240fec80f101fe8e574d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

