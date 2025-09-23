---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHG45VRB%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFjerAtq2f%2Bw95MzS4BHqIzXvVB8tVvGWTNZAzG8t7FHAiEAoT9PdTI2CEhPGb0GuyNwEyj1ZearXsgAgKBTrn5RtQsq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDG09s2RmhCd22egJFSrcAx11d8f2fRo9l4IT4%2B6umBpHrzxiTMWIrq65cXaFnjHgo2uaN7krKq5Se2VT7UGoUpdfYsm6DBkO%2FxaMVBVaVkmNmHRoL94dKiiPzlxX8ijiWkq1fupex2l4dM2yNB27mG7XVCCYsSs02nSoFErZNekH1O1Ei74y8bdd3bvIUvMWPEgZqZ0VmvOfjhc9nWhRMEI5NmqJG8kbeXa6QiowjjCpRxBvOTKC7MVSg16EnADmyezJ4tPfe%2FkO1rsqYIUnwWL88kGJVgjpyIkHumje0z%2Fv83aNZvTDGjELhS7efzPUUKsaJ5%2B4d2f3F%2F9BAwNEX9Yqkvn6ehiAaqCo9RkKZ8kijD1A%2BnZKtrgLRd26COdbgUiVIoZL5gOxzF2BpjO4jGnu1WZxxLURJNFpRxPMPZdifr6rMQTvQEndModOy%2BK%2FU0PasMuQxYjF1%2B56R3duvRNp0uOM8v3vZM9yYMVoJFlSrnjEmfG4Ewlfz4EXFyCacN0GRspA1F0QaarEG3lC14H12Z9XTdly7oaeE%2F%2FnKmiTjtGO6XYirZLOzp1zicPBzufwD13HymDQ3IDS%2FBnw%2BvGgKF%2FSwk3wfXOXDkdF%2BzNZRB5NgZaOLa6dTu7qb4fu0FuF4aVSY%2BA9ZBMqMP3aycYGOqUBpTQZZpkkuISABXhQcaYzBXfcBtoRjYk4j0I0wW4YFAYlrKn8y9Ay2%2FGIDuv5Wbe8VET1%2BKPRatVXyweev%2FpNl3HnkJsuTFK9t186SSWVH3GxvG%2ByKblyD2LP5tWszb5SpHDBItXmY%2Bh%2BolnKoaDUA4V245KCyPvCACOTShgz08mTxQ1wf2sXtLUt3Io%2FFOiLBghCEJkI61QkQ8ebblKskzdNH1lP&X-Amz-Signature=b8b75a66290291c617d241c145961443187de0f14e00deb03a9d0fa705815a97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

