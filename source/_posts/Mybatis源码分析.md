---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSZS5LEA%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDVXpDKmoNrWTuNx0JZODWB2nVA3tufYGU5REKnsHvlNgIgW4w0oalr%2BYHkTMLyBVhjnCsR0AlQy8XN%2F4TC8TtMcr0qiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPwGVd3SYipXmiWgGSrcA3%2FgntJKPw%2Bdf4hvNAqC3C6brL1POnkRmATMfq6i7z6wdrBoPaYo7oe78%2BzXXD1%2FNJlhc7%2FFpFHpoSA122nR4Iy3gf5BsttPPmwbmoPxgLszGelKe2ZSme3bZjQQD9TgWTVSa%2FSca1No%2FcGKT09LEi0wmlfw06AgL4KsByaax7BsCKQI9lf2J8ghOBuNOekR%2FwohnD1bAsOv3YemdVffnrAwXgZJ5A9m4XyaQ5yty4h%2BIDIDD%2FdBMN6fC8VrJy1lvNI4Xw2lNQORA15iHKSAtrS4APZ%2BD2tuqO0tacNgDVfVBRWEJsqXuUlJF6f2gHHwf1tkdvppwwOS1HaDLtlfhl0%2FM5cjIGYAd1lYAB%2BOZkvJZFTWbS6Op9MyowgLr7%2F9dGg1sGfpRuJeEN3D8b6XMEqS71aiDI%2B1Xo2t%2FUWdzaMyqM%2Bpk1CV1yCHaLCIRpjoiEQiy8JOPKFhsq8YV46hDS%2F7ao2My22s%2BbnX2gwlqpkVnfZyr7Nznp%2Fl2RW1q0RmTuI0elcTNLiIm0Vi9ousFxRV%2F9wH2YsIyyLdYOEs1Zfp8jiameuTWIykGhwe1zTbu5ec8r7UdgO95dZYyQ%2FhfU%2BoGM%2BZNBTSqDPiafLeiSMc35UhvwaT%2FTqPqk8vMJqNvMgGOqUB95YgvRLvqf5Qy8f%2By7JBrJQn8BsGDewBx1EeuCTUSSQGW95SSx2GiKCyhpif731zDulG9J0SRjUeUlibMzMDRchquBR53Kzh7KKWHtmPCpbHjqXaVlWPuLww079NgLueoR5ZPVITsEnMoRLWyGtkZ13ULUEwaNubWkXhr28ec%2F2cGc7bTBwMmhAuydly8UtvP7fsIsSlnpZLKWF9jsUjRdIhbePb&X-Amz-Signature=3f8a2fd7a7c7d12db75a3af8315f951e65a6ecbbe0f7bb8442c0e75328fd7e85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

