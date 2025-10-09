---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ULHHILK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC3NLLIX17S9kj129iUkATWdMw8yH%2B1h7ZhAs%2F3Kt%2BJbgIgcTUJqBGZXZIKIIjh0v%2FR76a6vMVP%2FMOPH5Ho129N1nwqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN95T42TwI7mpntV%2FyrcAyzz55Qs7d8T2MKFPDE%2FtuT1zR6oZvDpTfPBWRKNPejlBzaKiFi2hugsVO3iK6P%2FR7%2FNW4gjNdyiYK0TG403b48GylSJUabUHKH%2FBjk%2FhSbytQNiIXj%2BxFahTzWCITMyBhFCsZ00LbT78dA8%2BKcLWKXkA3V88KzY0zzTk3Tan43nJOn8uK%2BNbwU6v0vCQ357u5cEEPd0wHA17%2B%2BgEEi%2FWKD79Y5NAFo8JzM3SIDVjaq3VU4Wh7Tjsir5CYr5kSQOfGyoTOdwHcmVn049t0XNPp5gYSTAWLNiaylbSXuCCa49bcYc9hVZ1lNa6qukWWSPY%2BaJNjOCR6EZa1yO8u0m3P15IEf%2BSTa1mebQcMOdWkvVu%2Frmud2Y63nJFdqJsN1XOXWRgHR2J0G9ifr0cHFBQjeGIf%2FO6nfOn1khO2FTQr6gG3PXglGI6Go6Kh46qv9u6wkwJ945gSa3ugg%2Bkt6oU9g%2FzUVn%2Fii5WYnnh2QxzSrW1p2AIf%2FK4xlQ2a6zg%2F%2BXul824hs7rsKMX%2BPDf%2BWl%2BedhRYptYW8dV1rnZdTP0MF45k3z9a%2BQMNJ23V%2FjhgXQawWvjA1GXVVD%2FMRjM3ysK0tIjZzbZeEs2B3DUvA8JUv9ld3UwoxgX6tjkzxQMJfqnccGOqUBERHo6fYa%2BUTOqvOUc0UqWHkerl1002tK8a8VuXa4q20KY1CgMtpPX8%2BVzGfbLZ%2B21EA7sHTSOqBA2fkQ6I2vZoJ0d8bK8%2FuewNEB895vCW9i%2BEYyGsKAQ5p9tMM8mEQsYrcM1haIKE37PuY%2FsrFpf7IB2qozCSo8rtkcT4a7Ok6b1kfXuf922ExcAiRp%2BD1dRd4dsU7Tth%2FWHlMoqRar%2B5md6I6V&X-Amz-Signature=cb4a0e8ba22beedd59d76bf53cc7b33a5421b0bcaef846de5b566edb87bc1c8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

