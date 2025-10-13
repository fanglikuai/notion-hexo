---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624QKD2IH%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T150059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDnMab2tQWQHw3vRObY5htJs8mUKLYMdCHhbzfAG6gxYAiAZvOBUl6p3EqR5NKzyVioV6M%2FBSjkBmpXqtIB2e0lpbSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMFxMd5xMxwm94e0rlKtwD8152f1syQvSvkbH4K%2F7NUakhmllFvtV%2B5g8kIHCmtTjMv0tkBiYbOu5TFWs%2FEZJ2lf4VQ%2FG0a7AIC6SebZCXJqHF%2F%2BjPXMbSdQm4EBbuTu%2BwtYa%2BoTzNkXvCE%2F9SQAKrJfMFoMvSUKIKKtM7EtJrumLpncw7LSwQgpszwSQQ0B4wQ8YlMhpJ51z70UExecshpKOpmtw23lPHRqZDuczrLjVKdnbwE4SB4R0v%2FUtgMGPuXK1KeuYXsYqmdRuCR%2FRugD4AX8BjR9OomCpeam2KGUuHDiK2r8PHI3aUrf5RpikR%2BwAF5j1YiLM%2BQaAEtipAaphHg924qifCQd%2BQs6Xfxn5qdg0CSiKFg5UTm6xAD2s%2B%2BhS9dIUNs6M0ZtYuGgMG9fYj6aBTuSc5ZhjO5wvJY%2FatITpLYXs2%2BTUA3Y7JJNjYxy2GVETd8vxwkhNkNCrTE92sK7HPGghoanttIisdfxdUuf5yPEwTlbd88pR2RagZszY9%2BYINw1ZcqUraEOLp8kdO5OhqUGG%2BPhwf5%2BR5zDgNfUG7C7u4Ofx3rHpEOCi0%2BpcYHC8yBpb%2BaHlAPCi%2BpCESsL2I30iZhkv77RPPn71AmYrcRdMwOvmyMH2mImC6rZQpuq%2FtHA6WqeYwopO0xwY6pgFmXxk6dd22A98ysCXSawNsH49nGww7F1y7SZuOwAWVZkdFDCqDLCNc7S3BMi6p1x5odX7T32xEV01TgRH5VQRMEgdHSLQ7UlHnmSwbJ5OjYIaaT9kOLlDOufEmSht%2BUj6ETLElEcVOTlxTL8Sfr%2Bq9ACe%2Fjyk5bhHUN4j8neOpwxvN4BSxkiCPp2IcdIiSy0dXEiuF4ohVY%2F1MUGVq1Pyjof9J6vLN&X-Amz-Signature=6707739dc22d4c8551f09231207dcb44bbea88c0e4104b9dee2d414b017ab2f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

