---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ITRSHCE%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIBEJreZ7WqhhaIteyYyGgmnNbBOi9H%2FPsaCJE%2FBzJUuAAiEAyZaC4%2BhrCS1RHLDEb%2BXy5LFcL03OZp5mIzBSKeC3Jr8qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bgf7sZmB4Q1Fb6wSrcA0vYKgbOAyaouyLDGqgkMOGLC%2FiVE6GN933eSx4D7b09%2FeBodCSWXPkqeelRPAaMiqBODyGW4mafGagDXKlL6DANGbpnC1hK7wj1mVWLCTTHXFQ5a%2F%2FSMfIjLg%2BdBABpEcxc0qrBi0OLHiMMEX9AkupX7aFb2HkolW31EzMWut7ssJ2yL6M5pLC4PzXPo1AkxBMilJ%2BZVpTACh1BQG%2BJwOcJ%2B9SUP2ZeyroSWZRalYDdMYpmxJK0pRWdZRsFdHOWxDbsXKGJNV%2BAj3i68kP5TGDSQ%2BC8JohzFE6jWTY8b9gznEDuN77o9IcMAwziP3MPqmgrVqqntloZY8f78CJV1Jp5LAyx1obyqEDi4KjWJ8y2zSNvjLDpEflvw9Ds2iSMsBAy0PLDSxFco6J52pxLF9%2Bo8GIQAc75f1XTfUSqjSSUAvRIvYbWkUkc3SzfQYiJJhtZErGCFqSI6GLlWy7R9eLsqf0RPcBaiUJRyRQVWcZv30i0w4ElycO9KzOuP9MOZ5WswhmMLQL0flVinT1gBcbc425REsUxuALeYjCSIZ9BOEYss9cJnQe5OMjyosrMXpEr4h1ajJCUqv5Y5KO0cUS8cPRoIWmTu80GuhoNKH%2FzoGfuqjyr42qteVP7MKjlzMcGOqUBFjRdspeHOU73vkV7hdJAhdrlReKbu%2FO0C2Qj7Ab8%2BYJGslavnKXFuYdmgyDAUNxDc8FcQUSGBSQZMXXfk7eMOsDesWqmocI0t7wlUt9hY4HGaCg9X1AMorB5rcKVP%2FNJJslQPDPdKvZyhVQqHkZRGkjjs98FgQezdktc4VFe%2BY3lDKq3j2942silv9QajGYLKSKdw2pz%2BowFv34fmlEgyBdji%2B1s&X-Amz-Signature=4c699291db688cb168df3999cce45a9262b5a7f2ee98615603eed491ab401245&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

