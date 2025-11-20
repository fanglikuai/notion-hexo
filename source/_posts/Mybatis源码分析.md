---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJBM75ZF%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T200047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJHMEUCIQCoE1fNhWBtnWC0JHmQzLoM8S92itYuQzuN6Ofp5ilC4AIgC3s%2FuzvZwZGaVeJa%2FDKAs%2FQsLTuNOPsriL7ARvQGGgIqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBKCwINty%2Fsgw2xmGCrcAznTi2STrikFVGxLReelVpLUyJLH6NxhBjTJ%2BHEnmJwJnWBSorpkc4m5EBS%2FxL0OEg7T%2B9xC%2FLpLOgGs1Sfxf%2BOdC7c%2Fx5VbgtEqt9w%2FTjvKY6SYIIm5mpoARKeRn%2BATB2lIZSrFQ6uZUHPggMu665zcDQ%2FCrENkNPhPIfGGcDn39NO1prTfhXjWN1np11r%2FwiPrIqQrllBjLC%2F3%2FWMspZ3H%2Fr%2BLndrs5jWaJ2Fjy4ljC4fi%2FsR4z12wC1jvt7DouTOl25jymuhDnAV3sFNdicX6UGwOvKeY9TwgC5n45I8oh7WayLjHI%2FYwSUYOq7rXGS%2BpZGhtVC4aRjl3h%2FuF0eh1UoHV4sK7kxuvdvZ29C7XMYosCi4G3RGZaSEIIWLLBLijS%2FNB9QoxieuQffdM2hU6tCuEj2okaxOUQahcyTpqilp%2BwuwwdGGfEINRVfqgFJ0TYJGAa8iwuFfwQw8zEUM65z38pp0yh0%2FEa1SYh%2B2uZhCKRPsv%2Bz3alFACCn2%2FiDre%2Fp8E1deYFeZ72WAWxwN0FZvGKZMYmTcdIXiqC3kYrdoifmIEcc%2F3VPR9mbV71Ks7mS8qePCdaTHYrkj06jNH2pLrdjRWi0B5xcH7778yom5FfYi3PZc1g1tGMLbh%2FcgGOqUB5lPvMAO9mOE1VtWDG%2BGXTN0zFfnu1S3%2F91liaO9CZWZbPpHqYOKXEVXX1wNOrYQs7rz39OKg8b8RQfHSfrZwzbp5YN9uo4ec81aPhClMIHTRsxXXeK63V%2BxXEla7M6A8QqQUzFNlLFT3NiPy9pTENmNeY%2Bw27%2FTAivmjRCDQN78iZOw0NBUdhiJpHnxtKFlbLynwELYsTgwvmiq121NVAolexkg5&X-Amz-Signature=e03cfb3cd9557decea07c93fce93b9be3174c42a0c9a644c38264b9e11f2a8ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

