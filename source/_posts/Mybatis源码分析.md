---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4KXZZR%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJFMEMCHzOwQyS4SIQQhsdpnFAK7ZFTiiNX9LoW%2BpT35wqblRUCIBMigfHrQxq1rdActk3vKKM0vEFqOR1PrGdDo6FoXQQhKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgmXh4YZg6UDVJwIgq3AOLK731TxBEf6N8NzCGeDwmmItulXpYxCtyBiCMxvOd5LCY189loQFzr%2FCw0wlOtQwwN%2BOtcChMWGb3m%2FAFXkKJ7G15WQjHR2IpXWYLBY%2Bq9eapZq9M40PjmwTpNa8%2FcjaghqD3uRz5ql6fhThbaoDLOCu5zzY4%2BjnQQurgYiriWGd%2B7XkgfcMu6a0H7hPHPRmh73GYPf49b8KewbTLRYXo38OVOtTcdPLdA61i7B5O0qXMZMOWFYwDTztjcMcQxJITlHX2ra%2B8klpZA%2FNPomP3lHlr099alYwM3ea3p1Px0dvoKnvPHs%2BS%2Fotu7fy%2F9rYha%2BrsQZfGvOhakQ%2Fdk8vNqKjWQZ8wmbwCE5kZE7ce8Wutdn4%2B1xdj5j%2Ft6SaeuEUbHtArSOYx8HukzS8BtwRFUjBmKBQEIO3VRdWuOdeE1Hax%2B5GwIygl1qv3MAXICxCpgXaZtz0LzdGQ98B0TZRJyPc5s9VjT%2FbY1jsIBvj51Kyu4AU%2BTD8aWbCP0PDrBLf%2BFBz7dj5Q09EJsK9qoFQQGgWgeYi8qBK5NIzB%2B09qDW9YyWNm21uPsXI1ZTh47k00jlpYv%2FVSRbgZKNhUoD6u1KZZ3e4whmPk%2BV%2FZDjBjPLyyzECw0AKN0egrczDro6DJBjqnATQo6Cuvb5iMQQuckhbud3M9H0Qb%2Bah%2BbOM%2B5EG8ZDKOPt0OikGD7SOni4xlXziIHix2ESJkgs92J%2FLrXZAEmbb%2Bc0Azm0E2%2FEdS2XaP9iRcgJGEhgCHQ0R6W4VTZBTPJ10gM%2FFJBV%2Ft%2Bmdxqd1hktgLXMHoin2XIaghYObMGXtTxClQianq9DcMloIRzCxnKlXk2Gm%2B3RBJTGQW0toMymcOutCHagSK&X-Amz-Signature=fdd64b56ee45eec8c425d1e38cbd17ebe7d0d3e77c3c081beabb207a15838a51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

