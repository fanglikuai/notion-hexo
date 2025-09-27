---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZLLJQF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIQCQukkTyBoUYDCgFASMWelq186Bz1%2B5xYV7z8PsCXDD%2FwIgU2DWkKMHdsVSU1GVv1PxrD4BLaNRTrcatIGvgid1w0oqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLAIrdt50eXDBPcW%2BCrcA%2B5AYb79hPfIan3vyDfND9wB%2BOmb3ZLp4uzIgGc2YpL3XyPlINJS614C62CsCf%2FQBuArJgbOQHUfg%2FlHfimDGPHjUOdL0WXdTFnqwBXUGLYqoTmi2IYU9zwpUAh6%2Faqa75b61ZOU2sYtrYCPS2XVb6KDL8rNH2bjtOVSRanZj9eGJk2eQsUssXvG7ef2OU5Cr6EmDDztqtOklPwn9wbNzXb%2Fym%2FQidI5OwaVn7O9SFIt3yW%2BpCrRNKup8VgvYjcoiRqd4j3esXo03a%2FJDRsqK8L3IgQ32JmLpEM8%2F6Sb2T6PTzZ8gquMzo71dwhSipbzkmltAEgso1pqZ4H4s%2BVP3Nk9QYPCZAgwdde5m3i3GMwk38bj3rWAvT7cF2Vcac1q9CbzrTJPytT11FY%2FCNy4HrfJ5EE9zXO4bg7vvjOAsnTRT3D3UMCAtDNmlsY42NcjZ67TkIYxWttXc9i18l1GcnSt4KSi7jrUgCPupztX4i%2BAxRtbbLaLc5FPUmIa2gesn35UCz8ptNO3ylXE6g0HTegGTcGuA6oc4NfSFT7Qs1Xfrq1FItSXsnt4lphzVzdnyw2Bsb8qrUr1upC%2FLPAlWlxYHMmjwODsH1RO%2ByU9RLJJ1V%2FpxM%2FYsRoWcce3MPyp4cYGOqUByaUnIOXK6xLfWJq4qNrjlmxF92KER395RHzuWw%2Fw4ucqdZSKsE0Y2nWX%2Fi7y5aj7t2PQOQYkzu9t%2BFFFfkq4yoFk1zkXrtwcguTd44xX7mjD%2Baq%2BkVbhpTsJdkqlRh6K%2BoHc%2FYKhY2Qorwnd7pA6cZQwFMCUdxHmoUiXOOi9BzFpuvNLxynun1ezCALgwLFwa6DoBuAepnPaF8WVsNc4LcqQ2JI7&X-Amz-Signature=d780f24f7c2fd81d8370420127a54f5d64c3c3581d288990475583a50a97bb49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

