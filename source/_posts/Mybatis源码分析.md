---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAO45QYD%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIBfOFcxJDiZj%2FH5nKGElNa%2BI%2BajqzCrO2snqSEG6wv%2B5AiBszIe4cQI6vjKJTjhGMTeukf1hjSM2xPK1qwvL3u06IiqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKTjC2iu0IgYoGFL3KtwDXw4kkjozUCnKpVfmr4WgtUZwC1DfyLPv4hp%2Fo98zG3UoOAMe8RMuEayY64NVpahinLn3XWAeYRUUxrWuV02ITwC2hGx156GkGli8twH1Ze8AHLwTPm8uQYgMwT%2BcvnmiWaTJmsuFmKgZhH92bRRcWNMCW4vHEUNJf%2B%2FC6MBBS5NgpKo%2B1yngT3QzvVruaneHSDUZ9OKcU7Da1mz0CCEacCBjxg1YoloZigi50KHWE2j8PMasZ0cWWrNAffpUCR4satSJxO5vwCeuoPJcXpiSQL0zkW043%2FcNNaElX1RPDQ69nH34To%2FAaEvoiqAJx1QVvbm4XwD0jW3Y%2BPGGu6ZRoJXi9ePpJr%2F3G9KDe3jroqsfmhbnllap3C6WUhl8S92yMnqoUKa2VW87lJ%2Fj5Ih3J3O67aumqgFa%2BE2k5yAe4jZ3Qyn9PK9LZwSoN8sP9RYP%2BLFzmacGs3Yz7Rdwgn7wnzeMuJssOg3xh07gLM8g5tpY%2FvQjApquwoOFDOPe9vRpuSttFBuCj8MmtGwZ2vMqGGKP5nn5BwxFy5ulOPhTtunh3vjKwqsaHywfIUqIpVL4vFn0N56Uh6w6z6h3YtjlAD%2F7%2Fqi%2BnvWILnMJ61ow89RZ2eYyMKGtcnQDyzUwpbvFyAY6pgHSEXSdKF0W7fn62VRoT97i3NL9Y5tIUPhXuP7ybw6ZSAhoACZCLzyBbTYrJ15J5o9kR6U6b2Z04zElh2QiHGGj%2B0N0TRb%2Bov9Rrx1C4aDV8QjvEneFx1vXkH5V%2Bqsxuskzqvo07KPZK6O2aaoDghIJzhxnCV4EcTilB3ol8Oht7tqjdtP%2BLNwOq%2B9%2FmxUlT8vACtdXxvTNLmHPaLdGi%2FsRsf%2FJl20i&X-Amz-Signature=b89fe6b7cb700467c7af27c6d047710df034397290b1f508961fbd6fd11851b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

