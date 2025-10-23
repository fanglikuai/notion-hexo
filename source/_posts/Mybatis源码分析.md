---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667TIKJQRX%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAWXiLLrGPpWDHRIes9vK6OvZhGZyjhWjVDqU7BqiyefAiAyc10NWjqy7a3eRWwwu9IALZ5E%2BNs%2BjkoMYmZ4epL22Cr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIMSXsvi%2B%2B7tzBzXcLXKtwDocMtNgaG42lOmnQPx8lqbpWEkOPcfK1mCrB5h1O5pM0GjdecTLLSDO4uLdC1%2FOlNO3ShSlkKKCacorvdxIODx%2Fk5ZXXzM1wJY9drMKynJJ7QVxBTg9rb%2F1njd%2BIofBjH0JrDlDrC26JfcbUb11ulGoI49jACJHy%2FzUvQggZxk8clec2Z7q1TlHIFlLumcshoYRd2etS8PnRUfori1Sx8veXPwGRY7g1ZhUHo3ke8DyGGhVVAypX2okO%2F75YKUCfSaG5vx3WidfEqMHqiEnFawNSAG33yhlPBifCoE1BhW0yREEwyCUmDz66oSbVXwCYNrbi1%2BoMiGtcC2Azd0%2BGFThUD1eK8%2Fl2z2IIKeJTm1k5d0L%2Ft8tnwMXW4UjQbyR4lMOSz5Xh7GXLFx1cbg4yBh%2BR1PYvNwjYBYdlOzQUlRz3YqPb%2F3yJH4k4KrJkZZn6mDO6RfEB27E%2F6I%2B9F9v4Vr7712Qt2q%2FspS1mfGuHUCNq6EavLPv2Z8PfYOl2jIlzdFtsvQsLmiH5iTGZVil0Pazlk2qXOkNWzrfazDaMZgIvzKfnsQPuSczJmfxeudPnFIPWRmipbHNRD1ez2oOiPsFBVbcjYLjNdmFbZuXwVPWc00c98p1GN9G0%2F5R0whIHnxwY6pgGcyGN1x6P41LWLoc9N7mNhTB%2FY3tL1Ro00ac9Pd8ByOr%2FmZUIlAeyGr6DAj0QJxfn3UGPZ5hhQkzz1HkUJNS1G3tytVbWlUGh50jQ5ktjQ4a4njB%2FnImM6%2Bq6GWQDgM3bxKI8kccW8bdhd9oCpHSf%2FA0ImXpQIskLAsy2f%2FRUMh55tSDyX69jEcM5MTnabWVO21yCgvNsQ7A2cZmFM2eoriIFHYHdW&X-Amz-Signature=20913c6dc333161bcab7afae761759e1ad4f776790f1e2fa87b2dac6f387b8dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

