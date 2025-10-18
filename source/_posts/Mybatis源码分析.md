---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EA6WK4M%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T170051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDI9akIpdC%2B7%2FUmqehFQmBpVFMWJPjjPHbDsvtHxxMcwQIhANoGEBtgBH%2FtHl3qIb3S%2B60wGc7OX20Daf1Etdg6e8HWKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxJBFswWVvi0qbqNeEq3AOZthziLuN%2FRw1m06MYb8dmHcWkJL8c5PjRFujXWMAt82b4Bc9fCDVXvpAiaJfXgw3EBDNa8WwoU2zgLCMdUlY97ko%2FYm6%2BPPG1j19QTmFY%2BNzOZsXkKwLFEw5zNNyNkVtHIqQ49Z6lGY9s6xgFTB%2BDRpSiJg6%2BUa8x1mbKqkVTdcS8xhsnneZ7VFW7Sx%2Btrh%2F4dRHMgTXkEyYSaii7u7sQveUSbr1AWbaOwz%2Fvli80akSSPOwXLIZftsWfSZPCkTtZXVgZYYv86Zh01hkZuhbhPamMw5BkWOMLqUSdpbZGlqj63sEyUkuS4v7lx23ToaqoznpeQh%2BFJbBXs8WjYc%2FRC3ouP8Yk2MjlsPASakc1I3paGrM7%2FSdI3RHvhxSBhfPSu9t%2Fyv8bvuSjt77dNgOIuxQNWTFRGkj3SEdf1dQz3NDWNnv2GXcrhRuwRyL2wFSMaELRgAcsoz2DK%2BfXGlXZ3gfod59KheUAvXVs721lLfJ2d93Esp3dQkzWlqEYNqOlbBAnCjnzFE7kcEwlY5iun8vittTCQ2wrEtvYTMxP9plyjupVWDFBdf%2BTL%2FyIzZ0b8YaERYu5NSVoomCq4zc3MWnZx5SQYmkuwPFA2PAFnuzh49Hu%2FLEzAp2hRjCEg87HBjqkAZzEdsjza6gXvryLAA%2BnyuhwDievDPfmI8Ik9sdRRbTaQvEF3hxR4SePmXjPsMBk0%2FDLnQ56TYtSQW0D%2BOAnlGzgQHI0mPGAA%2BczI9Oo4jidtu97WjuHX73WN42sw5WH5%2BZaCIURj6jqt1h%2FLI3CISyVHvlELupKzvsbRCX9xbHYBDg5Uq5qroUjTk3ailORB5Gcxmzfd54%2FWZtrRnbOsPEnSXJJ&X-Amz-Signature=d3706072be535e5425df4c0390b81e6633d16457ede7dbd0bb9363d1d8857633&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

