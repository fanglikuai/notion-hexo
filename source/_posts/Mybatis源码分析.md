---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS367O7I%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICoEEGppcG6plVGGgS5STKfZpmLhC7v1RT38nhDjpXQWAiBMShf6tGbZyza3QLfwcMIkoJdb44NJKsi90GOwH4MCoCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMrnaceImw19HH7IfKtwDnwC0tv1KStQ3EQgi66%2BZvQDRHetNYjoCBySq0w4K8JCkEbyzBMcJ2x%2Fhr%2BpmMICAgI2o0l9qIbIq4Zo%2BOhCPL1c4yJBTR9sF1%2BECMTmt%2FsTpAG4tLgDDzrZkjX40b2RLi62hWL6pL%2FQNnkboR5b1F4mubTtKeAS7jGUScxwjBKZdrD1oyQGiWxKF%2BQDzHZn3g%2Bpy%2FX%2BWuLf8OqnlabC1Ceq5HYhg%2Fj47ZQGL5VAWiqcvecfiZEDfJnTkYsg3nU5KwES0YS%2BlhNGkq%2FoHywZV%2FbUgptV%2BgjLlVvIb0JTx8D%2FKdgofSs6bN5SIKBXPxS72BIa4qZQZO2hnMMINRyCsyvyKNirgZhdnzjy%2BnlDQ%2FEK49zswC3XPmSYCi4ygwBuGsMWDnfpuC9awi8PgEqwxW%2FVIwErNENm9t14enNP5qmSVBeMXFmlhaqCGFqecuLQQ3SsIEdFiSZNWTNJkYo%2BDhdOQru%2FwzpUsngq3QeQqNd5mvKSUDIIVpyK51tlV%2Fl%2FZEE70pIK2M4J8B%2FxHkO45JVZh9bXKmJPt0aXIhdziQcdwoPvFp9a%2F4KFaN%2FIORG37lC%2BRg2SEwWUeyVu7gwxLLRA%2BdZjUFk4SSxbb7%2FFzhS8AmN7vd%2BQO23spEOYwi9f4xwY6pgG%2Bf8MbrKj1D%2FVSpj0f%2BQpyiI7XQU1VI5aATzaX1iWwsm9rsSgNGDLoURNLN4KyRHuwgE4uXX8Hg2qT5ipfB%2BC2J0WZ3LJe2UlOPaEtqTZrbTOpXYHh6%2ByuBLleu2nyvtA0pEy10RUEks8ZaiujuozOB5OkOD91sFx71rltLZUZm9darL2MZXv7r1ZTFdHoITDyDkw5FfJQimTgH6iy%2FpabY2GKKpp5&X-Amz-Signature=ce3956c9f2a9b1511ffe688cc08386e19091a3a49a16df7431a46ac17a91ada7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

