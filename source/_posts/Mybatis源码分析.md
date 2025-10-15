---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHS2MXON%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGRvD8ENrNxPCfGZ3jYanl2MIPtM%2Bds06x8QqwETIikBAiA4Dz3OtNs1hDiWiw7NEQ0G9LjV0abiOBqC9iBDajbp2yr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrvhQ3lk6IPadb9QZKtwDPQmelvNkULQgE074J%2BoKDh9t%2Bcek2nluesalQ3AGtQVE4GT0qCFsb6fuKHSkRpbBTv4x%2BrsSuIuyV1M7fDHlE%2FYU3WeCwh976YodBnLD6HqmOkoShFPrIB9SSZcDWgLXYZ8%2F2td67GsoGRKHv4ojISk7HQUyKjtq1pMbYUxsyeCHCi%2FRRc4%2BPneZiNfDXHOQKSwBh0jL%2FtkSu05NDrOa0ODbI8%2FDuxBFGP8%2FDJS1IybqbLbNbxaaSnTsxzYt%2FDjaEJfdVqiWx%2BioDXwU4MRlJWTyj6RuLPkxv3P3lRlQc4DGiKPSDZmiKkMTeK5VH5P6vB8uKdQui4pQpx8mvq3tRxtDwOY9IbwBaw%2B45zuJbpbuDlGEjM169TTPe8zIbymxGyVDjH%2Fa1cuE47pjsniDpwdz%2FTrJa9QW4rtDOKuEpiwPpM0yWRwzYZwGhEiRrVWggEPLcPg%2FLRvAfOnZdF4OW0iRXDm84uzP7z4rkI0EQg0UUZBsI4zw2RYyUK3MOiwogVCHkUD%2B3heLXglVErgZwhuQv59F%2BqB3sfOScJiub4U32MfYXBP9rLuDV8dFUgYwkZ4v7sRGxZntK9fYIYD2tta7gchKv9XxHS%2BOrIGO4zpf0fj8bzwJPKo%2FcG8wo4m8xwY6pgEvN1trGwrmqJtliQoimO%2FlaykBXQBRAiQUD3r7xD6rqE6vrNdGLh0VpVTKC%2BrZx0MK4lbWV%2B2bzz2VpB7xZi2quL9%2BUsF6vs92HIP2DhLqaApfLksVH3DZrsrM%2FCvtQcPeoAzGGAZ%2BdcuZOcy%2FNhiMpzS7lLpF6Eajpm9BlQrfjB3S3kpN11jxdRqgW4bYoL32dxHOgqu6FZLdQQD0SFgW4KlqVobD&X-Amz-Signature=839a80162ba487d60a33688ffb6fe50ad3338dc3839dbed451662a315cc6b07c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

