---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RO3VNHZB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcwTa0ksD3BikxjF2Bu2s9pLyNSkUo%2BMHXQphScdPL6wIgUWtnWL0GYhOdtRiQ6zGiFOi2XKvz6A4MfkD4iQv8nvUq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBl8MRJzBQhh8s2gqircA3A8ybI9JzpDV07uBeFiX2x%2BqOML8zI2o9Q9Mz50VnncxWA7pkGt6xqT2MJOsGRwS5bb3FdK9Zf8lRAi7hL2PSH3w7iGyQxO7TFxxVgaPiNZddRpedos897dy2sQk%2Bm9Bvo4eaHm5d54DIKuJf5SnxYwhqAo0wb40jhFcAxVdWUwW8aB0OBkl%2F%2Fvgzr0HSSKmKJxEzhp2r3G2kIPJIhVhVWcEdFqncuCw7%2Fqn2JIahdaUL4f76f02f8ISDljPARRhv3wVzXwlmJQtCScDxZk0UPFKwadwjFWK8Z9yry3Q3ff78QVi6VkcDOA5Y7%2BtOA8x1euKQ%2B7a5lRhdO6R3pSaFL0xC53USIGO8REzSluVsQHKkxnJS4vbQLuBjXN%2Fd%2BLTfEus4NLfHAhLHPWhxr2bkdjeNcp7HvlSbQpf4XLhi5NL9%2F75GJlCfP0vQaxUUJr6B21ETxH04UYdIW%2BHSviWvYdrjGJ8MCfWiSaJ%2BPrvEw6Wtrn%2BmcxP1j5H9mfA5Jz56YFZXxURAcu9p97%2FOu9wctmXPV2UAjFJYlwqWWgtA21Dc9SapF6V73jUtuc6sltROVPePA7UB2kEl5Vmmro3O5TT%2BycsveVcKLVqqLEU8t2iKKceOvU96FR1eMoMPK%2B98YGOqUB6cxFCDe9EJFHyHBoshEOB19qPi8jhy%2B7S%2Fhf7TTGW6D0NrJcZehrJajKB8VgIQrevY7RQu0%2Bnaetuajhv158H%2BKrDBITPO3Pk0K0C%2FrnOyZWugb7Pg4%2BvLSKwbamu%2BH36gXKbHcGOMKirylUfXhTKOX%2B80On7du7Yf7mvgwX0xdUxWOGSSiSEf5V7xMUjKhHP%2FqkYw7a%2BdcTnO4pD3KQC1rwDMLU&X-Amz-Signature=29b32e86837cb2253a131c5a60e1eb9977c623a76665a22d8d84665c19043929&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

