---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LPFOUEP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIHMHvp6WrLV%2BbYv2bNc19yCboIEXP6yG5IdizsEjenFyAiEA5mYsEAxqHnoFFE9bsBy2upTMvQx3npHl2PKnYyhjrxoqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGP%2F6sN%2BVdDT%2FhASXyrcA2%2FuD1T2xVY%2B4PO7GM5B6HvQfoERlX2WwZoVBUIEoqEm0SwbxHIgDtsuaLHnT7x0gxQr9iSeC1A2IuRJPV3l3u0K7b6zXwrhXkcdGl%2BhG5lMZXFb6voNHL6tePlRZ7hc2hOdxUvl246bvmLsFMfMbaP167oSks3QomKl1mFkCvSMQYQGaoDnu0pTsndzcv%2BfvtVVnEdWngr%2BTamnHPfOfe9vQog6xNUSN7zu1Q4kbtbBuOa%2Bxzs%2Fp2R6Snb5ZMpxtCo7%2FXI%2FtUdTVCNaiaRucBJkFFl%2FHWrvWX05SkPAL7nRQlMxL23smK3EVIuD7TSDByPgDMUGxlVgEyoZC9Tp7vnXc3xtg4EmD0hhWenU9S299qxJ4rrVXTL4uVR8PRqKoAK4urJSrlqEcBVni31pwkaQUiCEgfa1G2T7Kgs5p5cEYDjc%2BCalB7%2F%2FY9sUQUVqi3mjeBh42fHZI3JLD2jzUId5LdJ68%2FrntKl5gMjo%2FsDP9mcAJXnwvLiIS3J9W%2BE4w026cK5l83tpyP7eFJimyCIOQC8j6cLOERkGkz75XZ3GSKmPlDDPG7om%2Bsg%2BBGq3zSrEzHxmnU5AO4MAEnqGu%2BUeiMcvxsWTN2jprggX0lMldp41QwSmNp3SfG8aMLi318cGOqUB1RDS%2FygmhHM%2FZtVx1aGjvirrG5aTo3sXnZ1Ps0hjZB7uxrHh44yOnARRJYtUDO0kIdOYB3MBlFXjfFXg8r2fDtt0wiuq8BQYUf%2BPXrVt8A5zKRsM%2F4fgM8q4L5VZIxWHDI2wA9JbdFgnfrUNzPPEPN5uJqBAu8A1nC1%2Buv%2Bb5dsO1ABSh4%2BREgXfSTyU99QTbrKd78XhckejMyfijNaiqgOinm7A&X-Amz-Signature=cd9533ba7b19da44fc4c507a72a95a4434eb0fe412c2611c7ffa9f9e20805149&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

