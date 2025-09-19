---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664D5ENW5A%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAd3UiecPg17ZGSMjv0rwIW7GhVVghDRWgXKuZrGPwTxAiAweoIPMo4n7aEa%2B2wONWhb71%2FitEh3sKaLOiNmnxPvziqIBAjO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeo5zRH9048khddh0KtwDvvY%2F%2BuA3ibXhNPOCDFCOXEH6ouljPeARKkwENc0mEpczSw%2BxG3GMsAVMaxsgpLox7KATLb58%2BsmMxRPpBfiCFdMFVprQdj5OgLXzSvCUef%2F06QEslZJhAa3tBo%2FCNj82Z4nWOLrB3ub4%2BX04JaiYSWY9Zlqit%2FF8NoqQod3Jfvu4AfeKNb5fOVJ%2FJ%2B1gf869PYpApuJ%2BZV59H37vLq%2B2Do2STs9qTrR9R6TWfrL4YKr1orzs%2FJ7%2F8Bn5RvaZ05QGlmBeIP05wqEWJZN3qMGX29IWOSF3DmrCMwqVf0RXfj8p67yfiIxlWgs3ixtECBDGLX8wEcUo88ulMKbjRSG6AZh57IpqUTqeCtmeIM5w%2FMVd19NKsfWgwuzJMk7%2Faf%2FmT%2Fd7K3dL0LTe%2B30ExXWWIqRuc9VL0uo0WVGBpUCp%2BQCDf5HnvFiu16GIM1Ctt4z3RP5WftVMW5Rt%2FoPCKA37F0fwqZDOmhp1yKl6PhNRmVBJCqVrXvKkV3uQStM0KWhCCRToSjOiAVd8873s8X1kFDFezIHvBE9jgakh7dSuPw3yWpgTJ%2BB9Rx%2Fm%2BC5LQSjv16m6YaxXgznKKR41WFDHGvsGyNJxykWFklMnptbAXJt2fATMVEIktrpKkngw9bazxgY6pgH4lMYk%2Fj%2BsANx4N1W95G%2FYc3lRCOMGN8bAtFBZ6LswPCZIdB4pwa6gKl3XWFLDD3pa%2Ff%2BL8Xxykp3725mB3Tsf7rQVV5DjbbHJQW17Aoyx3V0RdtMnh4MXNCniAM2WYi79omzQ0KSaJho2uFHEa1J1Epc7bdgkgulGxashHki9VwjLLD22lrw7Boe2wxj3uuENS5C9mK5jKbElH1Uo9CIDOOzgWBeI&X-Amz-Signature=de4cdb6d7c28c8ee4144e4df3d4c6ab48f6997dd079bd82b4908c53223ca105b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

