---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDYBDT2B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiAEKH%2BisnxWTur426eOIZZVLr6U0B7bnwgrNApjg50wIhAIcFj649Zkxb5soXv35lAtaSlqu%2FN7c1ySMWUyQfsOkpKv8DCDwQABoMNjM3NDIzMTgzODA1IgwpKqV4XNhyKNv9NiUq3APPCNgG3YeFvEkqoWA%2FuNPlox%2Fcslrv6Q0QYPBQHyXbNpkRbNYudwZJ6%2F99Xc5NQmLxYzINJlThVH8l59qlmpJqbhCcrlOHAdDa0ZoNOu4q0hrXRfuhEj9l%2FMvaeMTgG6a0RjlyaOwHf7M76jHV6ZunqLMDt0249zDCgsznC0NHlE4QsbuEutDHo1NmdokOtBqJtUbMD7nhnWWNdzUUx%2F3joVikPMMVgebfsS5tc8CV1aPR%2F94SNPeL0Dfpuh5Fn9mwFuXcO71psK3r7GAoII1xlMz2Huw0YAmFZhQOMgxQyxCRsfEeRakh4SVhGd57tzDRHRjX7JTij3%2FUK1wsytZYGRzX3LkU2ywJpyXHvQ5ndAYddN7oqCHcKRPORn6%2F1RiUMvMzTbIl3zrJGO1ThdlTHoau4RCUxkvL5oy4t2kHpXNA7IaguMTLjR2Cehb8HYf%2BREm2gdZvmGzIW9RJEE3dRbu3AIwrYyroRZmuDisBxoPhn8JcO7W4oTR85ilqopdMVeAUgIgtHXd8dSRks%2BMua%2BJnkroC1KjXQRi3mu%2F%2FTkc4i7hqveWESf0%2FuIF4ck%2FZePy0GEpCyoZlYLqSQTGfK9QLWbcPd0%2BKNFJGB%2BwT45L6xUyXHabHwMZ%2B7zCQ07HHBjqkAe%2FR8RqbtOAd9P15%2BDMcKf4iMv1u8JHZ8CGS9uqGpBmceixLrz8g2DBlj9XLvKBQnWqP0RsmQp0StFlq33770CFkwSUFDGNgoqDXnprQg6pSxxosMY%2BPWcc9vGc43ZQLdp03VnTRYPgIGeXh82JOWp5RebpWLrl1WzvfaATMiLmXzqUcftrnZhli386IxeMqlUojbzBSkhqrXjKTrn6b1hioJ9xw&X-Amz-Signature=f4b81617b597716d53cd41c01aaf652547833cd016e4b1a00f6f184fc10cfdd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

