---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQ74EODU%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGarWnwl1D8mdclh77jFfWHm2%2FNmdGirUzAworzc5KCUAiEAu47XdEyQ5yP4QTHUsiri8IErgXwa8%2BojrnP0oc8YjpYqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJkbRHKbSUz3UDFaiyrcA0Pf6Ou12Wt4SnwFkInr4hEatsRsKZV4w3yvU07TiffQebKeB%2Fdpch3hNMutT6EBm%2Bi03DLCsTd71youRMpcYPuCTenn3h%2BNRljig0SCLFbmI8DhO824SdNhDv9ZKJizOTwNhibv1s4Z2SqjJ8K1cKnu7HUVvaUqJzVp6x7oCdK1YX5RZL9QGlayAs54T4kmspZIE2SDGZwaFdiaOdZbi%2FDkLq87GN2cdID2%2F%2FCBUOXy8tM%2FMphh3xap2U7lKSm6a4AI%2FPXsFENLEGbXaZDW9k3iUqOEa65O9ZyO8Cka%2B2Zd6N1euD3xgj7TN1VJJChobK1jHXx%2BzQBKsdPe1pGPfN0jr4QLmd3WpL6RYeuqK1YS4rrqUylGQLRJ9cdmTWe3sonYgOHkXTeVWnkYGtw0eK7zq2RHrCXSW7wBAN%2Bxom%2FG4ePmJnTLyikRjRH9TQlVyQ8757dFAnB4DzUU9XPq60%2F%2FXhXHSX57%2FAT9MPIlG3pgz22d4r1qw6R6Tx0P0QPVl%2F9fwiCRpMMGYYZHDftcGmjzi7KIDvTYEG1YODHW2kk3ksQErFEpZ9Elew11rccSXQfKUvfctARCLaxb549NdvqNIRc0spUP8qUdVqnR9xQHCu7fFYXgR6vVvM09MIeiockGOqUBkZBUu4NGnL06altAfUWTTSbjflQu75GAv28Jhp%2Fxh6bIf%2BsFY%2F5tWYD5i%2BdxY7UYGGJw0DleQ%2Badaow%2BfB2YiXaS49gND%2FzrBJvd4ZtccD6%2FsDFkZH%2FueqsiMN%2F1naSH5HB7v3pF8bU4Dm%2ByTOIxXX8%2B9nih0dY5Qr3F%2FOrkhHZz9us20fD%2FfyxMJgh%2FXbWZ47G1TLZ6mhZnFIZt5uw%2BDhsSgZct&X-Amz-Signature=400298e9531c85f01def581ed2819a7c4f79f1ab8b7c455ea619c2de0c57a139&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

