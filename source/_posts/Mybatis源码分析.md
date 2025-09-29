---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUG4XCCF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIB4cBUcrJn%2BbpAKyoy80ofy2TKGmSnLq1KPqc71AwgFIAiB6XHgHpNVqTfG1JpIkb9623XRD1BOEzcgwVJUt04KAPiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDTVaYNEtgAWv60qgKtwD07Lhcj42XF%2Bt41WkoT5MJbzPg9zfxFh0buFcSCqFQjj9NQ62GXBNqVSLEDt7HSs%2Bq1PFBniIdFDVA2H95yqB8SSvuCmrRoP0gf56XrhXkRgU8Y3NAjrrKsQHefGlGic2cBHovrkCdFjKA8lh%2FpZI%2B2NnkXHVjSP1lxbHR1nQP7ShPbvUXCgpFieBSD%2BqtKaFzl3A06CffDL7OMzawHFpWUR5%2B%2FdqWhtDd6M8KIWQGhIR1X%2F7k1%2BqGo6AI9t5WJW0D%2BFiKWEQwJ9SVAz652d4ntcuKUDiOg2eOCLPOIHZCABn3iFs6fPXiXS513p3gFK%2BvpSW31aCd7nTVwuf56hUyO81f3klBD2aC%2BIVMuQja4Ae8GhikW8RXt%2FaqqO6mrLJTU3CySixGNFf4367XfaTEfnCSs1A8LCqO3ywmGHkUTLxcl%2B%2FrYj8e2Vo3GATbWRpcLDrgth11JSoz3w31xJ2NK%2F9cjPrESataadPX7BPLHLnpNwTILnrlwVoW%2FqfvLEQcdhvFx9qepUKEm4v01yJY3YWYTL09ZDfG1v73uEQQkeLY4oNpZwK2UB%2BW1W00u64X%2Fx9D48Ljxc6HTuK6zimFov2nEr8tRKQOsTqyXd%2Ft%2B8YkfPHy8i7gQaXb2cw7JHpxgY6pgF17G%2B0D9czaHOtMRSavywcb%2B2aWgRQXFeP0Gfrj2E65mhrFkmLzezAf9%2FxRsDQZ5wRk5zncaQA9SP9Yr46eyluu6BaIzgam%2FZXZ3yIt3ug1T3xXOuvL0Ud%2BeWF0d77z6T%2FlNx8mvYSt7h%2BHkDR%2BPO8VpCLrHb1PBKeb1bUybSEmqK1zq837BO4nwxk53LaryeVv1zqOz51zTUsTOLqqvmv5G5wWeDg&X-Amz-Signature=7b4e1fde4b2d6cd1eeeae52c540fc7c1bb706a20e9bf2af4654261c783fefc2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

