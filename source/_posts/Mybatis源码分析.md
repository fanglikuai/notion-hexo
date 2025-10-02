---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTBO3U5B%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWyJh7GcywTWMocmxSuH%2BXv3GJQHhDVDSrtY2S%2BABfWAiBOT%2F0iwBCiCgadlcUhwtbKq63ruNT96aiiPDC7FnCmgir%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM53%2FKh9O4ZHR%2BD363KtwDTASjY%2Bj5Gqz1K02kknj%2FP3I3h%2BYYqLWle0Dq2Yu6f4TDx1NKBySF4RRe7H7X8%2FNX%2BAg99F11XutzQbZ1uhr76AKnn7b1bI6D1nmSs4IO18nZHAmXojc4aDVylP36RQC8UsCHHhuVGXxqxPtbZk1FNuasZ3JjiquNkdVVjlLA4cwtbfhh9prkxkoBmm55YnXLCMCvmDJGAbJsYEZVHBSKGRgueOSvBp1IvNrnk0BVEUFobwJYbxHxGSpsMqwgApkriHxr3TRDd3qhBsTooL%2FIYlM3XEHjzG8hbSpGuD8VrC1cN%2FEXStg07hQh6fWgOpNJX3SRocaKd2C2zghhsK23uw8vsOPrZWbTOrdD0fKTvKBOokHQ%2BG6OjBBxGY1%2B1sbiHGyfgKCc8oVdoph3VRLKfz%2BIhhgRY50rf3Puj%2FFUQxnXYoDVU84W%2BxBAsGGTyqNlgnS59mABv%2FWyVXxmKm0%2BQyPmho3YQ9XblbdZwtcAOzC6H4JeKEpRHWB50DwIiiwsDUCv3DmIOgQMXFg%2BjtxFLKm0NfawRFmzeNiZTpj4UAvtH4jJWexDCdDarKB8BTQVB8TQBQJdFNYuqTSfqjc2fQ4BvxW8c5f6EPZ%2BDORKDDRprlReHVeKqgGvxNYw2ov6xgY6pgFAuitGRye%2B1RjNkUJP%2Fabu2Iop%2F2NgeOSy2MbhdS%2FTNWfG3z2ZuA%2FyZ4SWcGflxx%2FMC112fcqmfTj8omROtaOyuPP0nO2T48%2BoTfgOgxL%2F3eLHBceaWGyBXUPfli1VqFHI9WvUWMO0uqVQJ3laGeqjT4porwncwoLlC%2Ff%2BKJqpDmCGcDP56PwRsnGxId5Kop90kKauhz%2BG32cEZGQcmiaXpM4NUAg1&X-Amz-Signature=22b4ba6e47f93d6e32642927ae40bc341ccea00236b5155b06305f758944d070&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

