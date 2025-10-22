---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPJL47F3%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T000055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQC4CqZB9Pl9RR4fLgiN64dXgXZML2BqWqRJRjtWNN5dfQIhAPqvbL1JVRJ4A7mmYvTQbzp7hHwJU9qYHXUO7kggSzB0Kv8DCCEQABoMNjM3NDIzMTgzODA1IgyR6Kk5FNVJKKxSuGMq3ANsDJ%2FgdQ0hmhYyD99OZlpTCtxZExlCyny0KowFbUxlNFUSZDM1KBp1iTxVC5LvZlD5fSgulik%2F%2F8puy2qi7maOCS2l94JTiDlFBY%2FJ31tCpo1V27h3GU273zJw95OvR5TSDeR2sVo%2FF3G5ob%2BMj6lCeyGGTQuOiJDoYhEss2VIkbOSUGDSlRJqVHBExjo42ERe4N4%2FpgAe05qtUcsJxBxNTo0c0GYzdFx4naKuautb7DYBW98A2EZ2FNLpwknMMoOlLrrextw%2Bjo4Mu%2FsphOMEEvgGb1EIiqgNhMvfYKmB2cT8wFaUX6RDzv51ylOiPeDkpEQP4LAB5PurDY2xLhSH1bgcOl5nzzpehH6zL%2F0s1Zcv5VBb7%2FBEC9Z8p3vNbIkAEQlb177iFzCALnEM5nMcJilMMn8%2F6JAq%2FlxxvzYakNFEhfeG8eqfBYMiz%2BhN%2BSsveS6rFxt6fJBOxu8yuCqEdBcg7iXadwSKGqZwptE1RKJgPKmUHdqsN38S%2FUv%2FUyVFABSC2HqYVWD9iE%2BuIYNuGrcBfWQ53%2FClOxSEw%2FoqOAkmhBzGD7jeIfS2FfnVFxlqbj50nS%2FDI%2F0wYrgdXynasTr2KS6wFUD9tJ4tQ%2FpBOsZpHHD4mNWmm4nI%2FzDKt%2BDHBjqkAWXmGPZ2lRcRXJwIDawTNH536qWMEeOpIxa2LiTGtee2yFaQ8FMeUTX%2FZutI5PMrsvDvCOlsCun25XMrzLA4rJUbS%2B6grsWDev6eOog9qiKVq2AGsb%2Bujtso9j3g9vMd3%2BYf0ty3xcIWqRtM9WAo5ccLkN56Z3VtIpBI1e2EqIrrc9wBk8egRaeOKr8tYMOy29WXE4IUFuI6QyItnAX3e12Am22t&X-Amz-Signature=bb2dda02de96c7eafc9a6e893812a96d9233e9577ae8f74d30342ddaf3dabc71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

