---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHFZTHNP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHDJWIP%2FHFoQbEOMk2pCCA%2F9MLG%2BG1aTe5bs13Lj9NNdAiBzkO7LRgkFBhxCaEe4sUHYyz27idt2EyYz5MvPqSz27ir%2FAwgyEAAaDDYzNzQyMzE4MzgwNSIM02oqGKtdhhhCSRjtKtwDR1n5faGop1k%2BjqnuIbhPbzGCHRvjtQ98m2yP6Rfelr3LYG%2BWi5BbVILhWj0CLRfC1pVICoFeThMBtjCN3aM3dA1%2FeJfp%2FtGfpIZz6BDLvnuJQsD2OOnE6ulPOGgFvN6fTTOXZc1cfzCRg3Fd8Tp2tSTqOEDln3v8K2OjwlxR0dQfY%2FOLwgIJneusuuAgleTafmWu1AIDylPuAr36HSgQvh0SJT8tekoFOrTRrXYWVbdDmt2V%2BXDwh6eOm5Fa%2Bw%2FlBR2thPzMZU4352inldlPNOTBzHqOuuz4dOdJexwAMbJC%2B73m9ClLEN8zXyVP%2B%2BNciAAxHb8xgOnw7pc4wiMoItQQuwEwBNTEwhpUrk4jlOopAQdUnz7rOf9HVB5XccgEcS8HHtf0w6SDxvIICCzzL0qrvzW2Rz6aUBZ4Yi59pBzHX2Tgv4Mdvj0bIHVhdgEtXFXosrI%2BhylrLNAuSs%2FkJB%2B5NddzFm%2BnE1iGB1ZDc5j50MlzlL%2FQJjzA7bx%2BdQ82GExRXPjzh5k4gHvUB9ieQs9bMnQaNYDTgieouS7BfCaFKKzNqylvW69VXJpRe%2Bu2iWt40gwT0ZMR474cCWjwSApahTbrGRuorkwLv%2BpCFW78nFC7M3sDCwAjs5gwxc76xgY6pgGVlCoQm6F0a%2FZleU2WsvTViOyEo6541nl4zqyckZe4%2BrdasTs07UnhFiPChy4hfNFVyeSNu1UgbUzSCV%2FAYiq%2FIHwLa6gEeA7sfSw2dcHt%2BG9FmO00ovFDi%2BmUMIVLGmrq2DzX0oxVoyxulbTGYyDZjeTxmA7eAUw367qa5TS6iLtO6sgDplbEnBddjQuJZyvi%2Fvjc6JQvRXUg5AFnJuEqUElpF42S&X-Amz-Signature=4d4eb25d3d781d57a0eef4bf051bff57a32406bef723eea85b51864f1474a12b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

