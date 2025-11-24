---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X3UIDNC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCnxl%2Bs0rLR4TCtSDd%2FXaTlDpVMF11%2Fq0RjrrkWoGW%2B5wIgf2uK05r%2Bffwt%2FlJelbJoPzB%2BaOfZ3Mt8SZu%2FOkhSAPUq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDKczavwspzpoMJYVfCrcAwAAEt0JlBLX1%2FNxoaZm9kxqgTc1uA7HlWvyCBJQB8e%2Fzuaqvm1oBDKOl5K8iR70GUnKTePY5rhItPm0CrMQ%2Bc7%2BNKYBDC6t6Em9%2FCjkC6fw%2BB0wwwZd6A%2FqjIH3RgUMyViApQJbzUN2aXlGZIygkEExWnl9%2Fud%2BkUHSxiU5Nd7D8h%2Fowv175bZ3a5zHRyhjf1nHUkva8AY8yFQDPMipzKSnGrjNDKw%2BD9XlLWPg4BeB3vavUx1PUbBaGf%2B2%2FVCM8z4FBnba%2B9TklxOOfBxfC%2BoTtbrBzm8E888ZNsRBpYh%2BD%2F9gopVlTNAXlPbJRIu8oiH01%2Bvci8iUtt65bxT3QByBVDY9Q%2B0EaORwNCRkX54ohuIjohO8y6miYkzIIuzmaDACiaGbaltNWR6MLWxXix7hkNmmerfrq8gfrQUCPGLJUjI35FNjCVjHHkXANUp43b9jTBKr8j5XwrvwO%2FeVTRILGlNzNmaVFOZ1YO5vZircD8%2FOO4ClaAkcYY6zJin4DHBtK9B596XLM%2FwdaAGNcbQsQelYmLTdyiY0QCGrY0oFc79P5%2Bx07NelJXzT8KWFejREbfaxdsDWJJBtOSdMkPr1LjL88I%2Ff7coF7HZAktbbA7hiALvoHCExWc%2BdMIujkMkGOqUBaC4eFYK%2Fat1b3sCDefNK%2Bu7wB50f8fffK2kNFGiPqDOuQJSBUECVIycB6oNfQ1zutlVE9bo7eZvA3MYY0rEDsZwxAndGujqjEB12sRr7XMCZuRHX3UUmy2EzP0yE5UqRZfi5lNBPp6jbZKrumP2%2BUv7XDchzdn%2BhJbMktCDUQWKkf6avIRWxKAhhvAAsIXMFXI4OCuO0aH9KGKnuwbDu7%2BPe9YYC&X-Amz-Signature=f810acb46baee18181d258c3be34d64d8480e103e1647e0f0c6bf5dcf5819826&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

