---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HWMXX4T%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCZq56M8IyIRu6rvIWM0eMRxnn33edh79AwQrabVV5UVgIgDBKDE8SlG4XvM4plcYbAoEzoFj0DpISi0IKgjO6AzLUq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDIsUzF2Lc6uENZV2jircA87gW1qQbfxT5QedYLkmO4jB2fwBaArBiQ%2BqZA%2FDUAQpwNAiOT7H1EeinE72v643%2B0VcQXETEEV61nM9DQt4AGx9hvyXpYaYh7MlluztRDMyv0p2eD%2BaCmBUOxtVDIEyBe8uvbjWzITgL0mGv1ESks68pS0TDKpCEp3SIq1S%2F2iMLLH76lGbxZJwk4vcjTsSoPnl5T5Vd9PDQOtqfebvvpmlYRPLWjVi3ncgFGZrp%2F6j52sqkzW4ScXX%2BYug3LhBHs99nxwaML%2FSNrgTqqbSVYhSkJ3Ef0%2BJXPetMNLtmCW4Om4UDulsqVgcEp%2F%2B%2B06GU7DOnzPqjIoK%2BKjgaZQ4KJsPOafatEuM89f1aRrJFCkTbbglc4oLZQiD9tYxo2F0lh9CZIRI3GeGHn7fjX8kGhiVkMfQe4Y06K7OcRGMP7RBEtjEwR18Ifb6wzYo2UhGOu2si0VBGbSlJjajIBOLrX9F%2FUTJtcoY0ly0ynHQQyh7dDBApUFXOCRMPkl23nfYt4m7IbB1FYEIP69HKFsINQqQYjuW%2Fj3b%2BipvBPxlR%2FaNBEm0TRhp5JEQPEbZ80fvrZN%2ByIcBBGMb3uTJhfL0IkpD2AB7jyuwWkr5pg3LZRStdefVR2xn6%2BFHMVC8MKSZv8YGOqUBvV8Dzydmvgh8NUUXQI9W31v3it4WggPHmOmKsp5AVmraYEhJ7JB6ccfAldt9g4GO0Es5nRLwH9pi3pSK7Uck%2Bl57bm9T2Uz%2BFWSC20zRYsr54OZENFKnPm1fgW6gC2xsU4zII2rtSdFnmGHmheBcL4iCqvbO0XXcQSD0pP9P%2B1DSEuJEQAcuMcjKGfLEU12%2Biu0sGQEWw0LixqanjFkTZmugoLMG&X-Amz-Signature=03a40250e27a84bbddd98ba5a0999a712db82300f230513e5e89b7f6a29290e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

