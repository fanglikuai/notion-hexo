---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3IMNLXF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQCAjzIBGxq2dN2An34QiCNvbnOcTHBgnytYZcAkQy%2FfMQIgbYyAaGp6XEkFZO2aADNtsPeFcierZ8mYj2ZYuq6Qp%2BQq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDN92YpHRllxXQLzsRSrcA7xICYCa1HxYIg5LPn2lyJwojsrkiJIxeDgjbpCDgfxp3isCVpMvNe5VNXOIALh3lYCZlTqN%2BmJSX%2FC2IN%2FSVRTj%2B1idZ%2F0IEH3UHmSooLT1MDoMzTrKh3SHAyYmEyRkPT9r3q3%2F6fL9KOIvRmbRZ7YnkrkXOVs58KxAZkkss8TVvZRZ0PNIfEKnF%2FAP8HdX%2Bj7URcBOvGh42trZ1dZKwq0rgGYywgG5CHrwXOSNECYaKol1oflj8CQEMeEsjXSDyPVe7aer9zTfqHtUPEwPSiRcSOB62m30HAaXRBp5TeYL%2Fefy4tjfrN0RRVApdzP53C8kQ7s80d6J4lFsdLZVbooh%2BxRNbuyfzuXYaoL%2Fek15WXj9ynvbSjVPn2T67btEC%2FmSbYpFzIeOeQEz3e6kHapHL%2Bf8rF8GfFZ0tEqor0QuZ0xXipVadrkNVDQF43%2FVv%2FZJtfDHJ%2Fia7%2FwbFmk6lb0N7Y9ssBN3rTzofEmWGq34PFFG9z%2Blwmo1TF20rGjgnzO0QKzV%2Bkw1WzUh%2Fu2xBzmfeRlLUbBUrtXB7RPF9tQNwR4nCuZL3S00hRcRBTkc55CmSjCTrsFuqgCoG0IXYoFEHlyKB6TewEQFnoDDO1WV0r5xLlb4kIfp%2FiC1MNH2l8gGOqUB8TPDMgUks2RfsC%2Fm5uwAayyFouHN5T1%2FGYc4FRciiNtkwjNA%2Fu7o3qG4GEkhJprrAapyzb%2FCdGjIhZRkLeWC6wwc0XdoXi6x0ZWuo1cPZec70gkg2ZleFGvqhCZIqKrf7dsuTlOLTSP21ouIZGiNm9sbk2Lmy1kDZMpB4jXl5N5iTC9DqNsMPie%2BkV%2BZXKnpCA0k5JmWxZanotK1eT9K1HEvU4Ek&X-Amz-Signature=35363df66ffa4c9b2bf09bf4291bcfe2612355b9ac82c6586d2d969ed9b3a1d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

