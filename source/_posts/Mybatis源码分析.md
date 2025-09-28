---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXXHR2NJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIFgRhKFlk5Q0gDx4D%2FPPU5%2FSJTkMc9DrItT9fVanR3pXAiEA2BDyTIEinANyqAAcE56shyrP%2BRT4MbNTlDbmIEVfr6EqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwJMf1%2FT0TumRWE6yrcA2I9XoaPuNDypHOFDM4Q4jzEL4TCCDf5XSlWrM%2BX3UdxUrEb%2BrPgJO2%2BYXX1WIeT1h2wJTSjosT%2BbcNlxtv1yMas94CkguWi7hZ6edm%2B9grLLpEP5uyNDcyAK1XIyWS6PeMdMH2wTTWxW0URajllGosXAYr5lGeiYv57oj%2FLLNXo50%2BEiBLcdcUmBUitCdjzRGDT5YfJT17f6f9vooRDr10zYt3sifC1Sv5yEBwq3G%2BFUXfBNF%2BiGqfwN8Gi8jHyqzBB6i3jhMs9ac3oLYHlnQm8eeJhGuKLVZ%2BYJowA7HSmDsZld7EWIGYU5GMjWkEMEnpaIb%2B3Ul04cI%2BbLZoGr1m25lISbzUYuJxFGK4%2B4aniE9k4X8FHbkyFfaq8U4F14EkYbUSNmTNF1LO07COSXO3x4XrgeZuNLYUkD8Ayj69bjN8hDCg2Y2Chhm64MYcR0QRLtxM77Kgh3lyR94co1ZfTeJ0f1lfRqw04WVnQMMCavWC4CbooZp7Aa7%2Birfv5i1G%2FNEsiyttvxE2MfgHbx24UkmvWUoJiXoFJYzXdalJsKwUhMB1J6VGnyjAUX6R5yL2y0Oa%2F2UizIekypyRwta%2FntKCeg3Iij%2Fz0%2Fujx6h5zLU9%2BmSav2rFVrmlNMJua4sYGOqUBgma71E6KstftMR6HxZiix8%2BKScMDgqUQdcusAcbI8dC4yJDt8xie2guHKMIfoIf4sUT2mkVg6SAKitviHqk82eUlt4EW8DrAoy88zJRINy4kA8tvd%2FSaLDlQrcg%2BnIaAbR558S4z9sa3nZF1jjBl48HdtG3L%2BISJvjTkBJqvRoPjQHg225B2LKCBLJWm%2B%2FUjKXynC6z0eH%2Bs%2Fxh4Q%2FEByC9P9I1G&X-Amz-Signature=80101b05a879b3d32f99d6398f0a5bacdf011b2eeb3408c2d6f64ceb9acc41b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

