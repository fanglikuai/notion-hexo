---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYASKNRL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSBk4zWeQwo8EHt%2F2Nd2EByWaV7jqQoKTKoh9eMni5fAiEAs2quoktMihVxqTGh%2FWUcYZyS2wKzDgGINjXr4TTn8Fcq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDJQydv5gtgl60OyAGSrcA6YaPbGSUlcBvliwzaeqBUoA0rztqCG7JQKvEzQU%2FKJOqnhUumoFaGcMQ5KZPugr506cmHF3qGpI4TScGgPPfK1JFi9ccxbHKericcawxboJ6Yyc46pFYhCxNWubVGHGCVoF14Lyes7L7Jl8zw3C2VVoP81YTDCEP72VmjDDdXpKCRfIxD2I3qluiFuLAh%2FQsZ2iKcAdC7v7nW699iu5Ens76eIWTn6P43iXoJnpWaFf4T3R%2Fm0O%2BGFX4wShaW%2BVyWW0SBM89i56N5EULo4Khat8kh1C1W5xa4foyRkDwn7FMbNw6qWEMlZie0q9Fh%2BIfEfhAKz6A0VQCvGMMio%2ByNe2I1aeP%2FeElUHvlt7Al4wjJfSXyHpSxx%2B%2Bko%2BFinTvXOsJyj5eQ%2F5J08eFf7DP9cEYNB3A4nHfl%2FTiyixl2OVVVCNKVzDXUErLgTAZ6VAhgV1eHI65efKhPGOU6HlULZXETK%2BR%2BbITKX6w%2BgFyYrytpu4vB6YK22ZPwppsLRGv7Cjbe4urpzRSsUcHoKDV3RBWV%2FtRuY63EHzaBAfejmyxOGtwjIMpTf5ehV9xkjnIITLOhE44%2BV36kP1k8XoxJxdV%2FXxMe7xhJRb1g%2BYBe2CZwdp7mT3R9p1oQtx8MNHc3MgGOqUBXZiREP3LoE1aK0K%2FZEK82nuGGDfdjUD%2BMR60Q3ixVcCscHt9m3fzzujCHSFYO%2Fvp4WDJDATgYsqtG1Tdw1f1BujAzY8ZX%2BI0QdQp9xLy%2Fbgbk1eHnawBeYn8DOj6CjbtX%2BfUMhUauNEFaycAOBtJq1Q6OMlOhrCGo781m6qZJqnP3DGq6zT5Nkfm3P%2B2WbcZ067tc0JRyiql%2FEygBN0zVdi4Rz4m&X-Amz-Signature=393b94b2c4778134ddea108c0e42bb7ccabfe82c5c5a3f7de56d8f04d9f54ef5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

