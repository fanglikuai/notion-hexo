---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKI2V4HB%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCIANy9Au8a%2FnBMFDG%2BW153gY7rjIB0a6BB2I%2BypaLkw2lAiAW%2Fh2aeTZPqYi9VkNzH29wGN2PY3VhZMYyf5clfppnDCr%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM9%2FHqun4t974OhDUAKtwDOwHPCjj8l6p1dn3oRRaFq%2FwqV4oj5UZ3Ml4eMbp58A6Phd2GXGSXlyGKNn7KVtlnMOmBG%2FVpqvBrGBTmAKNiurcTPsf47JAMySkpKU04M04jHQxcbGmdkleusNSTVACLscIoGUT%2Fas%2Fjw3uNy38pxxxPz6uqjlUkWwxmWASWCj7dRhDLEQqNxX8OpguMB1erdt6YN%2FQgAWYXLuKIVRVCnko2ZjcZ9%2Be3LAHYyFgNzXf%2F5GKDz71Fo3hnmJhppEYqLjYlOByPYP3984R1xps9OU6Vk8E%2B9h%2Fy72FFPj5wsPr1Q0Ekmssw967BQOgP1ULDkW8CwtgZTiDp%2Ba96bTFH%2BG0nfpa6pHLWfZS73XCDxxmw5GKMMJ3oq1Z8T2UXXzSackS8fEjo6uzflaIeIjDVllqLZhfd0gfEAYW7p4qDUMK%2B5owRd1%2BdOXZhcOxRq1Dw8N%2FdLJFqd6uTJVJ72xdrjBNMH1NHJq74NqBDqaK3WfRI9XLXn9STGXtr5qyWOUriRGw%2FANHTXpvuhlNIz3%2FNwSI8FLjrh1X7N6UYjy62b%2FwFNPwJCskbKz7mHhKZOaLc7jsAOF4ZQEqrwtDHrWO5ZNGSzcZJD4uquxBRXcTAi0MKunEcPAo0oQeoGicwmbXQyAY6pgFuBN%2FoRqYofoed86TDW2AHW5ef8M0U5aNJzHujLqTgoujtB8NOo2JwULDCg3Ki4QtbzXi%2FwOZidl7Olthl%2F5rqE0YmzP7fWNBt%2FefVGzlX%2BL6Jq17al59yuSU8J%2FcNfrVTtY%2B5OGX8IQQNKXyDg5mIyrsz4eS1z48079Lvbprpfi9S%2BagETm68DcZ2GTmGIZdA7Qw17htcoKAKK2xq%2BZIleCHIN%2Fjl&X-Amz-Signature=94ba5427f9e0a3e94cb6b5e782c2038ad35d0be8e3ceb2233f9fc8258c9201fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

