---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYUSO3BX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC2x2xO%2BmKeemTH61HUjfCxE3KHJeUysfDC%2BlWhzTme9wIgd0a%2FFqfy91QXqbCJJt5rnj8EFhAflxstSTmG2G7D4j0q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDIndyxgXJy82ZBVm9CrcAx%2BdSrJTb%2BFSOeyoz98sXph%2BzrAWRdW2DLvglJUESuDWG0I2ped56ZNeL9GjyBsIc2lmT84JimKHpRBco5ZiZxYyt5hH34ryGDoIsJO9IqsEorHebhY%2F4PWROixrtfkledu4z%2Bj850uEDC3D0PFbVuCZAzUns4S3s6Ka7qWiGC83JkXNNT8ZR%2B9XbK8sm7yWlNMz0TDRFj7UDDZyrTvKKOuee6r7FARo1%2F5gNO5x7Vwx%2Fe5lpCzBWZr%2FqkMoxyn6CvZkv1BTsFXhin2ij9I3janKf%2FrdsNcWOvQYeY%2BM2dbkx5H11%2BMzHBUZ0YFzZAuZYnmaNVJKR4u5ZOtD8rHahxgfNEDEB5Lid9dKbeic8cUAD48i%2FMAVABhZHTuKsrsxg1LKoM%2Ft0xMIibdKB7Ca5h15fFME%2FXEgD6Lx%2Bg9t4jndxWJ5k9UfO2Oa9AOACvBiDpz%2FxXUb7NOczy0zK9xLZ9Z9NXEd%2BL4mzaIjTQclkIEsCFHUH5a9OxgeEOQRvc4KYpulheLXjygoUsSheTFFfnIBwTtoA%2BBiBcAm0e04lMvEMkwR219dyZ3meEL6FqnHH%2FXn8EHbMmLjUHE7E1B2uK3oZ5ofPy6SaGkVTPVtnlTPgPidIcOEsWTe8qEjMKCfpcgGOqUBFZXOmWlGlvXOU%2FChns6fxvSqNecXJ6czTJQdcYq5jYvOJ%2B8F2rTydAxZ3LMxL7z1rU9fhNC7KcoGH6Zq3dGVCZBbuupTRp%2BSuD5U6K35ArLkV8ozhKlRr%2FCQ9pzcBvMLPhtkbr%2BA64ivugXTjGtpjCaHlHdTTsktBr8ao5XvHhYwuXNPJG10nmP52nMfgaBVspS6La1%2BV6OxHycRtxwX4S6wyFg5&X-Amz-Signature=8f285824d0b6afc5c56ea598b423a38a3a02204266614a8501e96f6089636aa3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

