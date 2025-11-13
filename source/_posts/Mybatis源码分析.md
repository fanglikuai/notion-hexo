---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXRXY5Y%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB8kAI%2BtwrPXCAhCxXgzg0lkGHY4tO2NUtfxX571EvdqAiBaP%2Fi5z0EevPDW%2FjsSKGmSxhSHOgiQ5lwRhZEA4Wtzpyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMe6WBOYwRdBHYxp7yKtwDW1yLBg%2BNo4AD3oOVy8WH2BVlrvw08FKgIsS64VMxuWZJtXnCSAuLCBnMiZ03F5UNHijn81w51OZJyMosI5re0p3jNK5ayqCrUGjmFP7%2FqXnmfqHhWmCTGsHBOxnPg%2BwFCDspb8TxkHuFzI2bwGOHpPCWd5wwgBCp4TTAmEVFZ3TxIUFRZxFgV5FnDZiJgq%2B%2BbQUC0zz51iBLz%2BQ2WBVcBwHjtIf63v5POROzWP5mVuyJuBdaH5E4JeLd97xMErk1Slz0XVt08tfpDK9szKcVOMm86ysrRBfD0ly84EC7PHhbY3VbHkk%2Bwqr%2FCg16a1DIb1jLVZZ2g%2FpLIyAryl5tn3nt%2FknEpfvOndG9tiADRS8i%2Fy72dZtSzoHJYiozH77uXoSc3EWrtmFpqq6OR70kcVEti26QFweEcbeGWfQlGCpgKR0xkYlzURon3YTr%2Bi1EcUvMr5UdppWEcbw3oHYnLfw8oT%2BeTsEypjI3358ySbKgEDVXbnXXC%2Bt6j9j243MMcJ2vLSgjCJeOzQHE3hE41o%2FdM%2FiPez3dODXl8r6Pfcm1sLYK8TKwLnyTLmMZRXSHcizVR1NRdaMQQai38UXM9LyLrTVGzsL994eM944JXsV5%2FSFnMjkRrIuUiO4wzqTWyAY6pgFuUn%2FPlfnXNqW7tCDWSYOrd4gSPwvIt%2BWmk7nRwrmAw7yPx5H5EHuLMAWGsq8BPv5fndhNAZeFNw9mBEoZXq6rcAVAJQv%2FUpRkkP926mtUuA4lL13dFBWAYjHRP8ixaWR0OiwREFkTHlWyPXEK4lL54UxlW%2F7Z9TWWtnCVwF%2FJeimN5siELhsE8ysOD598sa25UKzlBfxapYmE21gRO3ovd5Oox10N&X-Amz-Signature=c60bcb1a3e27cbadf8fe03aa11606bbb8e999665d6a61f1eddedd5ff2a9eb2d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

