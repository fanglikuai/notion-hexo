---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGMEXELE%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNmcqWwnNYbPLeea%2BvpuaOWzQON2kBrlchvimgmN4WqwIhAJp5s7UTUhGl7mkc6MawemEYEY6iu3D%2BVrFTmYLEi5MvKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzgtc%2BXhm0%2Ba%2F%2FJOQkq3APp%2FZRuI6BdSMkmJbjSleblPMzWyFHty0GPbRx7TnjAgc2sK1Xl9a2bAEjyqkdstvDW5nyzZBkaUpFmgc%2FcVwXrKoEOdFZAEOMnE1eif1ug1uGYC9OnfZ0116hAATY6Rr9gjFqpyR5bHqqnmApXan8%2F4sSfsvyRUquhiQe2oJUp0Or2ZqOeutsJ69iMll36kq1tA5LfCRGj5TW7h6og6XMRpuB1QjxtRuZC5ie%2F2GyAQzH5PIf0lgKI2hZ5y93EsNx2mw%2BKDmSoTErxgf75WMspAU8%2BfiKsn2O%2BUCp84HZtwvjmTERPSlh7q%2FoY8uJzrVHfug5RKnYiDxyWC6QZpTIk3wLz%2Bywow99%2BHEc%2FW%2BAC0snCuMfcElRcDtZKN%2FMCNtqo1CSltVQKSvz0OcuCZF2EyXnzUgpOeNwUX7AwmS5u3FHdMvV8v79ZRI%2Bnq5%2FGhWYUveHRetXrdh1zYjjHCT6729AZySxQuk1oXgGyinmnxDOhwCk1WBaop0hHX4T3onx0pcn0YJlZrYo6jCcB3KauPnGLCMrL6oCpM2pCXjvzdYURNjISnZukh5v2dgNG%2BVy8J%2F2F7Z28Ifj8L0CDdwd8GuPt916tF%2BVwNAVJXl%2FIvUxHzpzEn6nYS4wjZzCSnbnIBjqkAagBT%2FjYiG%2F5dTaSrstjNXRzVr58hDthDRxokW5GjPu5zyFDWUysuwbZO2QRgqh3LUz78jFwXveSMn0Zx23T%2BdF1W5P5t4%2Fh084QA8WCsNcH7vCmfYHWZb8eoio6y%2FCG5p8cgMtXIUPjeJSN1gWURoQw93pTmYDJBgAVQrecAredRhi%2F7MVpKbN%2F9g%2BrlAHrTBX8tE6jCrQKrQTDtrXa6uecrqoJ&X-Amz-Signature=ee05e5988daa64b559d32c840aef0a425a54bfbc0f1cc719d933192792f53ce1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

