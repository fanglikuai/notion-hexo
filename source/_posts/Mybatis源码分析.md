---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FKD4M4V%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJGMEQCIHlowWt8vQTeAvLkS9VNGi6h9VEt5fdNb%2B%2B16D3pnYNaAiA3f%2BJ9tNt5PM4OBkFJCgB6sZAPojClH1uEy16d97haRSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkqcvuC3KQZ%2BIeK5%2BKtwDMjKIL3v81wftvfI6Il4QrtKUDWkrvcrm64nTtCghvo4RDbSsFcTHo53J%2FsUu4jfU0%2BxSVfex1N8ddkHMuiH7CFiVdsZXk4k94LUB9%2BLpGTVKq30dRJhVaKzm7OvTlxJKiWN6Ii6foft%2F7a5GQTOgkH7gh90ni43fWqqQF1dcfnmKLdPR%2BF4vuuBKxN0CFtRsr5xP4r17XMjDq0mSNwdOv3XGnXycv%2Fe4qEtCg15yJnfotrvyXxVsxoC5nyjQidP344sz1nndXEp1qkS1SVwwMFy3ZD7OCenVNlVGrJf9S2K6a6tj4oMr0U9E4UlpMfV8y7rkAq5r9cxF67zw3skTysQUQys8EloyAOn24BUuIvmnbkSr6UUH3DTMowK1pVsj9VBB386i1NGfjk3mRDarbQ3B%2Br3jiswXy24rc6NZYctRR84HrT6bv3W0JtLkq8tmeUFECOJTJacqqoVUxcSvFhhXOqRU59SDKOef84ojBcH8RcpiT3FsdArHi%2BtzRVyfrzkGJjg%2B6bSXqMDnU9l77EO3Lh9hgloYYVnatiEgKs99NMB7zE0VaPAiUHCFNVdvLfMrtEcVqyUNntcj2y571R5ynXHY77MjGl44B0puXCEoBoTM%2BuN0vcoDfrQwg9nYxwY6pgHncS16y1gC6USYUzBUWOFBaur5%2FwS4ypP53qmbbzWEK7BG%2BAwphBPzDO0iZJLEsoDDBzz0tRybdqdR8M%2BSNnFCTkijwCu81NDNhUMK1bKHDJQSIFdXMo7EL50ZRCJ4RMIf7I2H9urLHcs3WxwLt0N9Doz7BIDTlUU3ebeYlDOpILpwXDzTobU92ETKJSVIa6ZSNTH%2FAiKWq931TlYq91X3ezeMkZkw&X-Amz-Signature=0830355b940095d753a62f74e5724cc02719b84de9482c9f4e9aa9fb68d1b87e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

