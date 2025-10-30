---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677K5X3FP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQCjpyeEOm2PenfHuzYyWn2EIAlIhVtY4r%2B%2F2JaBHUjAtwIhAN9lmLfSzDcijHrRt3M5DRQKFJWX%2B2pFYP26kXqBsOsXKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnkS5M2ajd%2F%2BxqqNoq3AN2OjknvQhuTXfoymWXdODXvSEjD0QzUqi%2BwHqUbWwkIKyyEWdkzyp5aZXctqAxbkrR44kYFx9IGNPB5hh5ZHosxevVdqtU4D8e3KAp%2F4IqO0eNxGxPES9Ry%2BIpW1jI%2By568YcIN28ardfJN9INkZogZaxM8MuzJacTYtFoISvMdYYPdQ0slglSCFeXYdSbmmJX4FNq%2B54ck1GYgmsReu%2BskGlOs%2F%2FtPcMzQqkER%2BRcpmSFOe7ZSbJidv8XcBTdl%2BWLkf%2FF81QOLpBMCDkySlvU%2FcNuIjt5dnC%2FbqIyLOZJjkL%2BAtgCCPQINU58LvUx%2BrwuKmQl8aA0Rxy7078od27KVpbSA0bOgDblp5LDAKxEtnHgiKVyQkrB%2FC46zty%2BwvTqtRZ2JAmWdZDulxFdtIBUw1Pf8fADucwZuc0P2aDp63Abp7dmPug%2BGjPEfCFGOq672GDfygbS%2BnB2aRxjs6bsRZKcUSbZGW%2FN7CJihrHcx8xDd%2B7w05so1NBI0SZjz%2BZVhQ85t5HMkDTCW7fn9olCUc4BIbb0Vh56XyfHSgjIEkEOS2oESqkUf7QT48Lm%2BxaJLvtfIkcskey3f1Wfwo6G8XEe1VcCVB16vvH%2FDSM1SAvm8KGM3PLKQkXIGjDi9ozIBjqkAW0iZhzj%2B7Ci0WyU1BL3nhGvptIaOQFnLMHCacx0fgn29Mr36yTUak8Psk%2FzF5%2Bilnhj%2FPs%2F4xQE%2BnVW3W%2BE2atgkCsV6mUMYifUTPLt0AjbI6hLBYio%2FkEE4lvo%2FtYdUmVKW541b5bTZsYQq1U%2BqhiQkbX%2FoqkkJo2vmjLrCJkoxTCK%2Fdfo1mY45ejCcJesK88ekSWDpgIKUDdw20HOVnGQNoJK&X-Amz-Signature=c95598aa9a5ad67dcb377e9d2b8febcfe991560ef3fd9ec95321c4ff1e5b56a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

