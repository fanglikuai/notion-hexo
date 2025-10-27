---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HLPMO3H%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T200036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2BXuAC%2F0s8uyHkkKYjU93Jg73M70FD0BcnbSrCw%2B6VwQIgZhSyUNrn8LQ8u24siqbw0vkGMPbt0WSYhyGZdANPoA4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfFgCvm9ODFQMJ1eSrcA%2FGquoeCeeGSr97u5T3ZjhsOelijCcrI98HvRLWKoaSqqjoRHK18If95MCWnD7NFdIwmkYOBzLx0eraiXh089rrLoBhrncG5sE2doATHWdeDvZ%2BQFW1gUWiBspDfp0M0H%2FYzeGLVi6M%2B%2Bw8ueOBq4Sy28mOmSldft%2FdlJATD0XfrRBhthg44aCcL2dNJuaQNOdtq%2FfNE97x0R7OpQrnMiz9Lw5Us4no%2B73E60%2FSTjigiZoxknX5l9VJIOIfcoa9NkP2qNBExgzVAUEFoDu%2BGK0rczd7dB9NHVP80Aj7X%2Fdunm%2BtbIpv8i0i91c1FkydpknBPQLRCttXmTd94l6XjHuwR9xZT5xNSfxNIYX0p4ct1g7tPf3XdfcGEOLbKpmzRZH6EVg5nj5K%2FF5YwM5MLRaCbxPM6WzOkVX2trgt4Rkfksyd%2BZIlbXdvz%2BKixMFXeZ5EKv3QN5gW7PDa6W%2FNlhgOSuysQUBZVC7ZCBEAmsxvcChwGs6sQmmEoOpS%2BX90ClRihXxhcCC19ey5h6xatirHEmbGgCRoSDgOoAZ2aI2ntbYsde5Hzq%2Fki%2FxTLesF%2F2%2BukvabHexBYQlpnEp0zm2btL1y%2BvR%2BEX49A6vu%2F%2FEFe4bq5aBNSkI%2B2NBEQMOKZ%2F8cGOqUByYToi2fiNnvvi2IDU80GJ47dGK0LCdlKQ1HrzS1GGXHoXTKQ9%2F1i4kOSkpk9mqCjhl8WYFs9KZeG5IZKOzw3Fc306YDByIQqS3jxWv27I6R4ahoTuh%2Bo5INQm3jidCXuWIHX6R66cOmyQCvAUGE9pGHzGXLRetK97axY0wJMzcKrB3Kxox%2BN3bQeVR4GeoI26AXa1t6UhR6z0m6BxGtwM1QMa6op&X-Amz-Signature=5ba8a4054796be7394f19c200d0ec7d1c8d64520b6981f46ed4f329d9e79175f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

