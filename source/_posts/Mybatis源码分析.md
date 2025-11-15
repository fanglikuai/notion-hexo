---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VGZU5ZT%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T120102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaSoKrDHp%2FKJW6cQVLPwrvdh%2FwtbgVUooqyH2r95j%2FegIgYGo%2FQkFVTIU%2Fb7WfwzasSNcdMYGmBKo%2Fb53hzcQs1gwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDOdVqn7KsbyUj2txoircAw32S%2BoUBQwdZ8SJ3EbRbUrnsFdtpHXuHp%2FX6iakrsK%2B8VVZkdmoiB%2FLAAjEVPldHgCMOQC8bvEeURGUphRRAoHF%2FQwkPU1uVXs7vCV8DXOnLcf1ZSUKzJXZ3ZPl18RO%2FwuzwSqHlrbeM2vvSGPdwBelimRuNr3A91C%2FIVKY9LFF0jokqVdjQMxLo%2FHueoUJ3zrsp5bb7UfK%2BeGs5k9dtE2G2%2BFDOqxLtseNM3L6r11b%2FAeReKQ4DCKx5gYyHPNBwz8Ht6Ss76JFspr2er7UC1eH32q3X7iIycAGKvsT6Cp4uw6nkyESjHwSF1Gs5E%2Fq%2FvWmDAVyHvgNhTkIikWUqDYMm8tjdtpcubSqpRQgOMHSw2CDLgAUbcl%2F8imDDTBMMgIkDBa0Q%2B2L%2FFhepdC9CTSYUYy64ELEZ3D5nFXzIzsquTAtJxfnj%2BBfcWzqybnrDN1Fjb8Hqs%2BmkkvwIAzXBNFYf2gVwh5YU4f6e6dwunn%2Fuo6%2BBYvNlYfNM2L8sRHJIgObZEbhyy9%2FWRNIRsLpGTuszrKDx9EYMpfJaJOYAxSr3kYd8tVFR7zGRwarw2E78zabl27NKz7g32UZOaH%2FdalYQunuE2Bo2T1jpyhwzuozSt6oUGCgDL8VZVUXMPKD4cgGOqUBo07YkXd13fkxf8oB%2FGXqi3lPO64S%2F7Ul1UbmOYnFNJFUj7u%2F40Qq9aNHo5kiTeA8oDA9qLsLYYQJ1wnKnXoQWALOkcA7D6gbOs7gXSe2SaHRk70r0Ze3fwz61GRDI5y12kz543eqcVQLaqKR0QuKv%2Bb0O6DzUVoLQBUTBcDwfmpguHpVLhsR94gtrt9Avkp7GnYlsCGlsKpraLm8IDxKr%2BcfHXyd&X-Amz-Signature=3cb83af475155cb85fac32bf5cfc22d965dbaf7b915ee89de9e1966b3507f340&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

