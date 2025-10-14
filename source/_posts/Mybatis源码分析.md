---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZCMYXXK%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDH%2FQe4GIth5PkV%2Fx9z1i9wcCFqRsFjp%2FYBOPPuaPDPvAiEA4We7GRgQqBB1CumNVOr2NZHuqSyRHdinGyviK%2Bs1PC8q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDGiDFEPKP3st61Wb0SrcAx37gbE%2FOYKsSxLlEZm6G47wyDHIcKMJvh6ehXMusTBpOhKXwYgnrQb%2B%2BbVZBGgZc%2BI%2FPuetwypoHrLqcdbSclJBJmfaeIpI%2FYBX3G185q33ty1DU%2BJnsp58wEKmAVEBDU1ubA5fX3Ju0KSknxMCp7Pvb6TbsM7rHZqSXpSSb1bOlOmYzCJgq8eeHrHnSgOo%2BNwp5BfGlt%2BDyE%2BlZB2TaO3U5Qc9lhNpki04j33wUBS0uK7vgsmKqIHG%2BIkyvNb7zblw0E0ug1ttDaKgXNomjua4ZjY2ejCiiqPt0oJ48aad6MjgnMDRCYD4WYH0VHQwi01Eq0Ha323VZQ43OGMxls2dNIig75l7%2B6JDa1bJbx8uVzco7310FEiZBxLX6gYS5z%2BO6%2BHcKiDXx3NYzJPA8LiHtzgfbeI75VsIhmREdwhtwIM1aFRiMMnQtXg7urH74aupdYRy7EheUPsmIMH83v6bfU%2BoZ26ii7GFU3k7aerTj0CZOMSvVY%2FlJMZz42QvCnKv5HD%2BOKJFRD1xcTGMkP8GiPPTodqnZC6U60f%2FaCCtLPdJhRvrz18zwROUTgVl%2B9l0uKIwLuh0LV0qI81S88UtE1Zl5Sg%2F1EZ4aZYcjcqiDB7HUm%2FSS13zGi%2FQMKLRuMcGOqUBAqndfdg7e0PpqrAJBvg7Lo%2FSmy5%2BEB7vZ7aDx%2B%2FMAJQ8RTxQws0oXwCZYZF7VcoxPpedDAFlqFopS0UXRw683%2BhOTXU0TirCnQ83xS7C1WBm1358%2BPArT9QnJ3gwDBnBzEzPNC9PK2tpam69djh8vQFrPd%2FAWHpMZqkfe1N7ct%2BMYTQeWGWQJxqXYRmOzZ0KeVKCf0I67mvdaktsC8Y6SLd91P0m&X-Amz-Signature=1438e999e55175e7838a1b530e2bf3ef3d2ad82d9ff5e3a673c3be6236b002f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

