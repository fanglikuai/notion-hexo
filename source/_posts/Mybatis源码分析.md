---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KZIM24W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T140056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQC2cVIuLw%2FQVsp7JMG6OGB13H3BKgK3hJD%2FFopL5K2EogIhAJoCyfs8AE72cEyW%2B1YdouBptzY9q1UAIIMOqEzlh1JtKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxfPXI69cZBDPSabpEq3AOWGM2iEUzL36rSD9jqpvk7RG94y9uFdvsLxMsCI3ot8g4psLBaQ%2BUrFBeAxpj%2FmXgSXkK9nNWX9EQU9sIjsqh8q9%2FU5MMz0JGk8vovFGWCIiVCqN5v1dGbxsTY7pO7ChJC04wkVif6uwMshGyWavcREUNmEsowsAFBk5plildcj%2B2v1AKRsnSIWfleF0MAtrn8SBq3aMVRE0TDspA2uPJJvITQ4dBcUQxOqz87BDGW3gsq3cVtZvF7Oekvwmageur1ak5BEPAhh8s6rlypZKiMLumOvocL%2B4BnK2gV3Dml0V6rHyh6AWhiLGIqkfBYbzc%2FbUf69%2BdaSDZN0tI89Sv%2FL0mBzZDoubNmi53qgKZ62iX9nGpl%2BGxD6ijPlSCVTlA%2B0sdEWOeYi4s%2FuKdfPOSlrXJMdqrLnExPA0nJMx4xX1rxEMyck%2FxLM3DMQoZenMxC2tDUT0Rd3fxaou%2BrPvzitkfswMF3CNrir8IXHeHDgrkeWPBXNKUwyYonoSXca92TnI%2BhPp%2Fn1ghntusyqVvRgHB8o34NWs86XcAj8aOUwCAyxOFC%2Bau01GbADrwx1IaJ9%2BvCbAqck9MeNKAYrsmJ6%2BoXvEJnmW5ntzNg8R%2B1K%2B1cMTdAbTowPYWePTC0jbzIBjqkAfV5MBcBo81yMYNtbB849J8ctYBF0ojeGqcXv4grzuVGW0A%2BgqWY5w9PvsmDK5YsLhfSMVuEtMq9ZP3tOP48N4Bl6TS4IRIniGx9QOHc0seeszs%2B4UnTbE3vRApr2iFJXu8fk9iRLLrxY2e%2BNmC9sUJH%2BbBpX14sy9Sv%2BHJCaKG%2Fuod1GIjyyp3aJuKAsnr5J5CyqycDa1%2BtDTex4FTRDzlwV6GJ&X-Amz-Signature=2721c63d1c85304481bf7c3d84df317fe4ecbfb0f3dc8ec013b6739181ccc0d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

