---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UOKHPHCB%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAVOJYToJ80AwPuNzgqMp0cAicRvSSUC%2Fb1WXAr3RdU%2FAiAr7GjXbe0RR%2B2qZrbcyc3CuELPYNUF9PEi2tlCAjaBrCr%2FAwhwEAAaDDYzNzQyMzE4MzgwNSIMUG%2FPep%2Bf3aJFw2NAKtwD9rRCcOCmRbxbPXGIhu8Lry8f1QG%2B39LU68YdFCL5NVcXoxCG7HTOCKqj%2Fb6PqQ4MLdmXQuKSoJ8SjECs7mU%2FOHadZA0BeRJkx4stOeW87rO1HsQMGSW1tnXTmYY2lIzBRGBG%2B1zX8jvD1UDVOFFvM0K3ig30jFyjvoBkC8PWZnSACM8mCLjvHsZd7ZqUHjR0kBiQfpELiMfxUNFcHcKa1wochIlZehdzNPPR7%2FQWP1T7Xa4ZVD1wwVv2ue%2Bt0ktxcAeoTJ%2BhrwnQ6qTE39ZV20kzDLB7f16JhYF2LDsjpLKbffut4PuH%2Fk2bLl9pf5KMcgXnVfR%2FPIIli2jCqABY9qYcBTy%2Fky54M7AC6wDxI%2BUYT0KpIA42kQnOyWPN6XosNfPeFrPXasaRTGHQzwywTRF8JoFHzecZ4oUxctgMtcAfhcUR9LuEI2bUpIwhNqGdR7uScriVqtXzKgATNMUXZpoYazAZIdq%2B72Ch8AaQMPKJs3yqzcSZ5HbBnwobk98MSKX1hOBXRyAQuxArQTeJX8nfMu%2FnFOYRlAwbMG4IIJNomAJP0p5xwRWDGfywZs7KZnxK%2BfVDiCckyGFkaPFwSOzfvnb%2BhLPofNIRowHpBxzGqwJnzmqggMwPvLEwqYGXyQY6pgEiBigg2oMXik9KkEKaG3WLrzmyJz1q3JscqEO5WoVRnTu9PBeuWKv37Z%2FKJzW7iELcyetOtdhCOvV56MhFDA5gueksn9MikV%2BfjcOLrv7VM3r7BD%2BWCXWZPPytRS2D96k3V6QCuTvCR%2F6NwuOoMTnFRt9jv0jhP5Rm2peuhsbEs%2Ba2ASIxLqA3Dm61QawfhRaClYDq9Qt3ERbms%2BEU7VC12KKUz%2Fy8&X-Amz-Signature=e2d4889898ffbac81f84a8d8066ea0967cc11fc39761602e93e3e51608b612bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

