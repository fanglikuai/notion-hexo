---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PHBQRXE%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD8iRHbqgVnAIgHEuN%2F78dkxBl5ncxk3LqwwaRpReciRwIgBfRfyikeBhrc%2FqaIWnOsvcfgtO3HsJISUB7VXmf7MYUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDHyq3g01uojACBILrCrcA%2BiQszjlh5DhWlGxlX9HCXAJBY%2FFXQf3X%2B41VedUIIPYuXVg9mf4hpTmDallJEqxE6mgq5SVk1iz88iTHtZLcHLwOi6kj%2Fh4qtCC2JqDF23hXMicf5yRYD%2Fps9bR0q9uITvz9lvBNxtmUXnA3Q%2BgeM9mE99yxuvZ9b2SlVIea%2BA8HJSRYl38eiz1IwS1%2Fy%2FpqG%2BFQMe7pG0grpHtajH%2FFOS%2B4fmzVrBnC9jp2AohcZh%2FDYP8Iv%2BmdvQj17pUv1B5l96L49RES1vds2ZILt%2FmS69XNtvFjZoTgjcNxgWbbubJZ9r%2BDhimgpkt3Q6F8mSeJyA1Nk8Dy1at6L2pRG6HSnWgD%2BdtlUC6e69kjs5%2Fn6AEJDJoCpnJ81OOJHOySPHKkOVPfDaV28cDWDKFj7pujbFOg2o1UWMTJzBatOp97%2Bectgboo7NGaqEA0iAyK5dGtmgyOdI69BPye905jN3enToWZflIH%2BoGONKbBgEckSHzVq8NQsraEkl7osR6zv%2B%2BvpwRDKL4t98PjnB1R0fFdUoOiJNd0pmZOBXHAGeTWKQLEXxoG4C9SWWKUsfLaRLAmwZFxuHRSSn2zNWcMYk1l9IpXWjDQtLsIhYh%2BrEqDd7sZRvqT13EqGsxfnsKMMGPiskGOqUBANAAJtQp8Zn0Nbz0iuzfhVZS6WCksW1kB5%2BxK%2BihniwKo27uf8fx7xSL9zV0D10H1FLbp3UtXiosaAFtGisvq08cB2YKNtJQnMHmFB5s75WMTTKmbO8wgalP37TXSUSTPnzFrqtiDjod0%2FVDpiYBrhZ7zP8wUzsjNjPcnWqDy7Xey92XWO6K5%2FwJoWx6%2BC9TnoTLYlwHr4XpTeU2HCCwNXSzIGl7&X-Amz-Signature=48be3859d95d77fb6a1e59e17ddea9a69982f549ffbbff156ead84ae58203d0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

