---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VRJL2FY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDuw%2Fb4BrWq85t1jP2CX2L%2FKhdOcgM7Bf7BTJeLhLoQBQIhAO4ZXwB9YQuA%2FL9rxfCMgx88MhYPPGDlaNmd7WopNO%2BAKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSPVGicUItyeWuPT8q3AN0Cowtw8HGxt7yRa%2BAX%2FFWzmSM%2FVWt1%2F9KkWjgq7r3LC5g2CyOxh2qvpevPyDtO8isbGtkm8mrGQEAFQwbooUGSe4nplHTZwD8IE9TokngIV4TrRVbe0Uy6R2i2t3RWez0hUCg3h0bAEUUv6UxqeT9f2KBadOL9RGwyJlEp6dt4IQOLtNdqRTzX8AY27fp%2BOp0vyYzCfGkuHULahXW1uFBd%2FR8KiT0o5o25Mo1tIMQ7QsmXyve5gdPx6GiYxIou1DwzxEUqkSPD7Iayz2OU%2Bt5ub1lX5fFcqMz8GMf23emtHVeZxgOIol3vV1eBfm79pbsd%2FA4%2FzAcKgLo88TCaLHRt10NAahIM1ZVWFXlt8DAw7j2vc8cXrko9BX0MPqavcw3oGixB3uir8Q3oXtdCpXG1rDk1R43DLY6Qlo%2BLotHQNF4OGBKKcEnSLR7C0zldQF0MCW%2BdZEmgs3%2B0I8iVSlhHpUlgleWHHO1r8ul0ZGZO6GstdpZLzGLw%2Ba1UQjg%2F7OxvkMkO4ubmXIMcv0%2BAEnyTxtqGRf%2Bw8AktVwtnbT0xo5oa99%2BsSdwsrT6j56eM1iFcLbRbdyahF0ZIepF9GHFNkmNehspBKhpOD6OHkcxbAfHD238fH9eP6snvjDJ7r%2FIBjqkARkydrzxazCcyVIL3duf5SjP5uxYT8kmP6%2F%2FEUTGXCK2%2BS0ZgFINLqx2%2FcKVMmYC%2FDMObupEOFqky4o6FitjKxInT254B7xOenpjEmDHsE7Db99DQJBhJVw16kTL7K%2FNm88AyWID0zC6yIcVAtw4E22vSjwFuXltFiwoSpHkuX6vkLGDbRZAptPqIPDKw40eBGVTXuI0S5KrIU4ic6l0dtHSZcpc&X-Amz-Signature=c4ddf9998165536d871b09b6d8796d2b7b396e868ec27f6f90ca71d672e8e392&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

