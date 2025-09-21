---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7A3OMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIClwxHp6ZgmiznRIWZarCFY%2FmCMK8U0mgc5QwPrTEt0LAiEAgoHV4GxokWZOVqafks7Uw%2BRfkRhDihjZiWeO%2BWkohZcqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVEZv9FEGH42j34uSrcA2ERTX2AZt1pLiRAmZHTWyBsBNOU3%2FOJaNRMRj8wMdqZ5jWccoH2Wx327yCsCdza8TjzY0H4yRG1cb517RwlxuqbcvqezDGE9bjg%2Bbp6983i2ilS7PLPb6LYceDWn%2BuR2q1tjpKCLsViE12foRASzOypsZMUkLCAaTOfbigZ4%2F52w5QwMcaCJPSX8SVBEWFRB4qVIX2xlYVWRxDbi4liq4dtCEWZ2xpjuQskALPi%2FUStLYPPVFUgcE5zcZEBgkgSoUhS0sDlxNTrcEyry8v5RhQpWWnO4f2BsdVRT%2BakRvqaOYp3Ep20hQs1lKLnBbO4OXJL3O2Qx3%2BcsZUMjJTr6mdvdwzHJeP1EN6LVeNcBpbiB3nzZu%2FRIamSK3Tl2S%2FO6JI5BewKO2dPvPJEgczk%2BePizpHAF87I3PxsRUkGG1FZju95UGAWqzFSKJyqZ4iLjIDq3D77RgzoM%2Fz%2BTnlQ08M3ZhnYbfQqcE5JKWucq395xf6L0NLsaFEo0SuCGv1kLNykFPBW%2Fep2n65okzJspaD7ID7M19%2FD3wnVvRnaPJeAxpTmjrxoiQ5y4HlPqQu68Vb%2B1kK5jxzOIR7Zh28hwRrzu6%2Bv24bY1UuPw%2F2Lhyo1VWadkUkPP39WCQSrMNHvvMYGOqUBlmq7cPTowfyP%2F3UjLFzTbAFo60I%2BrUbLSnqon3hoZVAb1CIK%2BDP8p%2FWSJVIrG0EOiiK%2Bkjtv4FuYiPsju2Zr2auaHwp0naT8stYILD45nJQ6ditChTXBP99Rg%2Blg4RYY2hoak2xLzYtH9iKqwQzzmlYkNzdMNxMggyModPyEIv%2BMlxKQ3UpI5%2BKZPzc6U%2Bxh0fpRmr0lviDXQpKftdd2tnLc%2Fghr&X-Amz-Signature=a24520afbdbf011f98fb245db63f9d3d050922a9829b0eefbccca7be6055210c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

