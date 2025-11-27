---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVSZWIW7%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T220043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID78FlPBL%2BCNZT2Cq2b24csmt%2FL1CHhm4TA%2BWFVhsAyfAiEA2caWhvlKyg4HcIYMR4mqFw%2BrCNj1flSfJFwzxigdZCsqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCcI7WENX4ihWw%2F8RircA5GfIjszmnYZmLR44nP1SRq5jcmQlz62j4uT60%2BYbXPTFKj6NBROKgxV22JS2snpNR7Fa9yiuB5VVsCibir83251K6GUVh%2Br%2ByMtaXLYKwSF1XE6%2BENKDSYB%2BSglPOc4uenpNogUjFpXDNYM3mTnh5eDvmYeeV6CKoIOY6go8XwohqHmnmYO3cgU4XnC6OrYyF%2B2yIsDs%2Bc3HXB%2BZVfsT0KsrtbRA%2B%2F59cDq%2FXBona%2FHvt8qVzaNLiGvQVawpR9bkNPrtr744CONjG%2BGWobiYtyAm8Ax0e%2Flyh36IwpTuKxCtaogcohmiF%2BO1x%2Bqx%2FAeT2Q8GDnHTcpexpJLceeAjgoxHCfTCB7V992Etnh1yyjMdyiXtzGX7S%2FGlsrxQY5Uzt4FzeLSwuYtxjtwqOgAVFnrnwTRbEo09EADjr2nSC8w5jLMkVgvwna31HAiifZZO04PMvBUYW%2BRsdbUYQvq84LN2XLKIqmlu8IQM42jgXO8QZoQJ9OzFym0d7vOYjyKgeMASf3xhp5SplcIf5ZhdFLzb6LtPZVRPDPjvnmrqx8DMHuipOase9GQ2JBQPJObqER5JieomXTU8pSPgQB%2FVjd5ISw0Dnzyi7jG%2Bdpg7X9TThWJ9zlDSMsTDXU6MLW9oskGOqUBna7RwoaBjqsWbYdWyPWX5MF4PmnFNIP65RnQoFkFujzDTH2X2aqyozVqz%2Bknzh7CgzxXx4uqZKlrlMWNYuJsP86jOUjBiD4Mb4ogUf06z5E4ZPVMaQuUVBJeFZ5qt58VMcL4hjlQYjGgUi1BNHb5t%2BPYsK1uU8UGMY0MbD5mkYPkGEyk1x%2BDOLZJfx0jmk88xc716pgTMoku66Pc6c0y4R65ADnx&X-Amz-Signature=81a68f16d50dc5ea0c42ca900f33742e8d557bd8f7da0fab9f09fc9ff493718c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

