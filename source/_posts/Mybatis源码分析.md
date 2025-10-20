---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIMTCP5B%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIAUQLq%2FsI%2Byp8M%2F%2BNG2GFjSMgP4jI%2Fhq6kwLcwP9ieJIAiAAt54JIjX0P28IhQyD70PE7YDlIk4iPYjKtyueMTRSkSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwT8xFa8OjIs2K295KtwD2wz%2BWiTD5iAtK2OlCIJuZshM7J34HSZFW%2FW4R3BM59VWZC8Q1dJgf2zGF6bgzP6ao6KevSsPlPwqf6qBCiOpon2lQX47HyQNYOInjfiKX9VvY0vJZou7yXyUMYyWLsaXQNTciRgmYbCcF30om5MA7oPP%2BN0y3C12Qnx4Xi82Ex8T2FVrFi6J5FGGNvohLX0xKSbuqRER7yYHeqr0Ms04Ah9WfpTSL5%2FtWp%2F7n8Dxli0iAUdTPji10V7JkZwCNcYffcbvp1kcv6snXHjL8Mks9Up0T98HcPR2uM%2F123gPULnXVZvmkfmOVvG83eiN%2FBdwj7G4bK5%2FrzeC%2B9S2t0EcGdFFsK%2BkeOHvnJIlmzKZVQAMk6c7WBfmFUyCKniz8ykLj3K9zcDli%2F%2FrbvigM0gm%2FrBNDFR0xa7FrY1%2FJBG6TYuCIazBvg63ZpDGVWsCvF%2B6p0%2FBSGd6CH%2F0NfVdj1ZyXRgea3BK5WNGLMgrPopgwsODn8fmiC1NqpxPVGAnqfWhvwl0vVgjjpJwAY5LhIOI2zOlbSvpDOtiEC38HUA1T0uKD4dZki6rZ08esi6%2FHc%2BI3M01s8KrDBWxYdYYa2pTkgKusf3XpxEPWhAU%2F%2Br63dPnbEZVm6J5QDQJUdswhMDaxwY6pgGSFYSWYhfu4NqOelkaKCuOvIm3n%2FLuavmRhRKMwTWBaCmJ96wSoJVTEQio3JCjAiDRUr%2BaKXWGhUa34dkX5crhsbGFk4xWcBa%2FmuOOYtJTrKHJutj4f1UwwJfFmbFPTABk2mhXdvLZxduvbSIP097JyFRYrmj2vEhRh8KVUJYsmLMqV8f5O9xV5Jq7J7gPLhqmeRyFyTJS79%2F%2F3RIg1%2FhoMZBSXZac&X-Amz-Signature=27398ddf485e8ce7af43f323021d1d059265a804d82070ed86ef0124b450eb4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

