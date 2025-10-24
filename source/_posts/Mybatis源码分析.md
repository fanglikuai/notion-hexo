---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UYHHO2%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6zB79FmM7bUoYLQ1UGVbK0kr7mPhMjnJXGaiVlwqV2AiBn1FV%2BmVDKEN0HCbOOUcNGrspC79qy4%2F8SAi1lsPCJoSr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMc2kYs2EBz3xC%2FmPxKtwDVsX8tRw9QoAt7KaOhQNw0Z87SFqmGODRIQ8L4DyvkwPwkQNzlQclx8Kq5NWhrZua12d0KycwXGK6hFfNrPnGd4gqRKFHoGJZUIU6rkFp4PXnsEOBqx4LqmAWwlOULDxFqCzcyrg%2FVy9rr5dz5KAUlq5z%2BXgXNQQDL590qOIjyVGNbAA7AE4ZGXK8crEXQzhOXBquodac5PHaSmnchOOXHFyBPXaRlKy2ZUevc5ReDPKuW8Q%2F5Obq7bY7UESygFTZ4VTKSTrMH9yl0gGD%2BWnN2wL4F2Z0uydn9kRATCdOXOx9FfFXu6ukcA6LN96I2cs3rUtxgUvXFmAvQ%2F2aHKSLUo1n7n5PJjJ04Hf8zhJHxyRYnnRVPMep3tzFvy0KlfDRJSRnSb%2F1cwUgLu7DCu5jZ7B9m%2B%2Bsp0KTWk5mIt9uZaTeDomwX2k5RkiFYA3EuOL52oawTjU4l0938WAyTE81oSVRzh%2BN6%2Bzer%2BZ6tPq5XMfKxewM68A7qaNjcD%2FI%2FYs3yXHT2MwsGG%2B9I2O5agTfcAuVW5H8GX44fuoCn5HEs5apd9lBapKrGoEoMIIqRB6qKsUi3MwJwqbu9fGx%2FqeBIasz5MTmkZPihvyY0GcYRWdIrB9U2GWcKLGliugwxIruxwY6pgGK1f5F1fDIxtSxFE25fzGfgJQakoEAhGVV4GulpbRjP0YPuqhzlV7SNpjFLwjmbxwjjbwauZ9kGqu77LiyjP9yq5bXt6ndEmvh38NVMINirHpijcWCb5jc%2FRYua8NBefh06krARue4RB6QPIyroMb80VpI%2Fs%2Bq4iAPg0VvTA7TRSoPGvA8j2%2B3XqDytVtVIFlpKvaIfnXQtbDGZLDO%2BW0wguFxYPUc&X-Amz-Signature=0bbcc8ab0ce3d55130149aa475d3bd8f758d9f841a3147e2666a8dbcdb73d668&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

