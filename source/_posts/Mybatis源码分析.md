---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCLKWJWB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T010054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDj7mFiNCdL1yEoSikNhLhF2G0YmOuEDcCDxoluihWQbAiBKCIjY25oZUdhOoA8E8Ecq%2FprykYh6v1gmmfUzcPwNFCqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjJGlAb%2BDcFEzEtarKtwDNwIsCO%2FMP5T9jcMQBbCINPTMpcnzLPybTV21C3i7wN7QtLLlX%2Bwf%2FhbAQGEBQT%2FGCFp%2Fe1co3IrGRiEK%2F3Kt2hLNLgNylHY5CdGUgh%2BlLQzi8vMQJT6yij47enYWETYgg1%2F3n5bLhJdZGIaD9%2FEn9qb5JuT%2FwpkrXFprfGk1VMmEIAfXrOwfgFzEZQWT60s2BJeKTw2MAO480nmojxKBiIcuH%2Bucn8XcRTsSjep83PyT7%2BzMsTZNVfBDmY%2FiM6%2BTsgZoMdAy%2BuABdBYahHCOQmw9y%2BiHuQa3ckUHmDDgwusqKeAVUhaeF0Lts1mYz8mfHtfMGACrRVM3i3uhFIECu4yQjSdh7%2F2qAf3l7gx3M2770KgxJPLZpW48yAp7CMWi%2FxLEhJyXJ7OGCiI9Fq4m0%2BumofWQqF9XBXa8TopOfX8Au8i3%2F7xfQWyrV7gYRJ2ZKoRRjKLCjlpIwA1oMA3e5%2BW877FSGsDuBh%2FIHWbHZaywKpRxJyWYSQBGAbHOsLRo7patT0SnqcgzemuMOBjOj5lbKjtanQmWvZuhDyddJWLgCGcQl4IKND0xYgOotm5x4GJu06WlrtsXLXjeKeVwyNemcWyxYxX3io%2F0uqj1GT6PIb5qefVp9VECr5kwwvrAxwY6pgHOE7czl7wIUbsX1CAHxJP2Nh44KQXD9loeHd%2FJFiEtNZ5zbSLxq1iHstxV0Ejsd%2FVgAKsYTlNPRf8B%2Fk%2FV2eDQhJINMkoo1Le8FP%2BOC%2FbPpNmJV7MUg2SddzKzlW7MEwfYAbZbvZ0SVgjS7DWir80fr%2FMmv1QCE6zujQX%2FiWbcv47LBlRsB9gLDN15Mu881xrcc8XSyEZZ3mj8s0WOIJGpqiu6UZ5J&X-Amz-Signature=c853ba8f2f623230d896f45f26426c3e77375a28016c60146d5c1892721d2bff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

