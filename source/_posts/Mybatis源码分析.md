---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHHOA3FS%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDIRUB8nYZYEyKRVZ7HFGfRArB77kGD5WoLnjqX13zdogIgJ3UNSqiA8a68DNYoadkZIzO8zrJctQwV4ClO3%2FjfAmMqiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDESWESoSG8%2BGS%2B8hBCrcA1U4Pvq5YYUdmRRxaDXDHQk50IWcleLObckLL1%2FP3ewqKPM5hcq3ji4jHypSQnSQZFF67z2g%2Bxp%2B5ckTxn1Raj9khTQM89SCmUnq%2FKgJQk2q%2BBMokuewy5lKJWp%2FRiNzh8nLJ%2Bq%2BjLNeHPGgcKIdQuJkQGuou6u0se9yBJj%2FtHKkt12tlF0EMqi4C3xe8qKtoug8J%2BhipiVn1%2BozE42%2F0d6MmeKqzD91T6%2BCtt%2FKdEWeolProFDKUEvIwulcEFfmzDnLnQBKaIf1M9LVvOCv3uQuaY2VOajVqjCftg7oJi1gy5XTru3M7BmPzzV%2BO1e78t3pzOKPDgfOVfUCaa8RoMcqC7TEbwH%2BerE95FN99%2FCVzku5EwOG6EOYKt%2BG7xdMpMSuHNElextxRFPZtG35lGThZ96TqSmoVy5U4vDd%2FfYO9lKTq8Yg6dlZ5%2BGb2oeKEVZkdFgddUEGKVS8kTzJsFjrNrcTuE%2BDwIiPLYpha6bXdA%2BPkIp0utuXbQzBCDXckNCi3UbljQZHjJXbRdzgZj8jNQfj9t7%2FLj%2Fy9WtJkXG6Mag52aoWd0ZK89i74q86zrfgafAA1eJIDP3u2%2B2Rn7NT872YNAMYtEvhT9AZRdn%2BGg1lfDY0AFpM4jhOMLycnccGOqUBOOvS5UFSTebw1GCuZJoXFzDBEWBZEIVoQYK9HKIpkT25jxJzv4NYxwSsjMd8qCUfxW4NxjVmS3K0EfN7HKiR1V4Ko%2F0IjoV3CKSb%2FEIaMSlXWaQ5cH4SSjtNwtMfI1%2BIWJ%2F3%2F1txKNv%2BIE4G3DV3Km9V7zKkdSw1utag9txL0e%2Fa8ia9AnhbCvDSfrAbrPzxwstr%2BsbBTMpsvagNLoetyww2gPrH&X-Amz-Signature=75736af6c1ec82f8f133b0ceb420b7285fd822b3bba063843992f8a30a2d9779&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

