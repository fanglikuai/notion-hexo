---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM5XL6SG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQCOujSzxtEUyDhsQ6vs5%2BxikXEia3wsejTkOdiNCu9bGAIgauj4dAe31bn2p2xWtohsecbuMwnfwRqTmpyEw0ItHG8q%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDIG4DQ68s9LLFkVUWircA2j2fOP0aQaxGZPU1jUBnTAGH%2F%2FZWoE%2Bt7w2c6vz%2BdG%2BXpt5zS7elTbQ9WZY8jPkmLkDcCh7njoqASNqRN70eV%2B7xj0cRZbimT0BYimUAYdE6JP5fC6v9WZgJWrW25Gsn5Tu241YO3qzH8WR%2FGQVNuD78je4FhW5xPKqXXN9anv21ithkedst48kVUxg7dfXKI%2Buw4eH%2F58%2FkvCMoWd6IL2Afb0s2Bevg5sBmGIIwXfgD15hjv%2FdUL6L%2BVhqaOAFDfrLF1dH%2FlH8OdzzC8Dvivkwizc%2Bv%2Fu%2FksrtErslAnuaOCzFaz3cnFE0grYD06wYLBvt4%2B9CEGagJrWFuIGyOUfFigQqBH12ZiX2659mkWqyMcC3J2M%2F9kiGpJaes0aCri1eodgvunx7NIcXVRbcMPo6nAJ4ow%2B7JY05MaiRphYbCI2SluBisf7U4z0bDq9PxM6cxYN%2FBUtSMpnw6FNhnRtNFAxdFDwlMLhI9UaZVgms2OLpYLcy8ojJ2wylX%2BbCuRvy4Zx5PlXWlV%2Bvy3%2FjI4CbN4G4r0TY7ZBNXB%2BgE3SSbDaGHyO5xqv8ScvwsrFvsFXq0uiL3PqXE8WmtF1ZR8BnXb4Id0wig17Tgy8ha0xzB4U2sCglyP8sUR4pMLGYxsgGOqUBc3lwpWvrnKaBB2a3NAfkZyqoOW4QYbLSfClkB9HaRA0psO3IjOFepxJLlJO9kUey2mEu4ceysmP0t7E6rwDLs4ZaNZjbH%2F%2BCz2mZXCXIE%2FOVDrWWMChgzzISFXvi3m1%2F8Dyp1jdvTlzha9wUSRt4NIuTzRjDt4xnAAPxBWo6SbmxV%2BaVkcpUvdp5%2BFdqbyeFJ%2FbBMqeKswjgRIVDPvmDxbjx%2Fhsk&X-Amz-Signature=9510227774bd641c5e026a9dbf9c873301cabce28d8fdb96102b20c171ff949d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

