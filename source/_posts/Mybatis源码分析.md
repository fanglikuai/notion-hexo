---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF2HVST6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCzX2OsExSMtIQE0tSxHsTb7vFOfrERNpH73al6I2WTlwIhALL7iPLSirKOuR7w63NIX7ooX%2FvvtqFTcC7XLxcV2QE%2FKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzrUrdRHoZsPDpRqfwq3AM90W0lO6jhy7NAwLb3Qfo6Ho1U3J89JNpnKzxl6hrmMqYmm3AvVrGKKDerhy9GbA2JFZusE11fDyd4pAuxKTol80S3wgpYfFRHDUPHS0aT4jtRw55PnKsIuiHvZvRk5VE7igsyjE1prNp790nSFh2C8vll%2B8N%2F3%2BFZ%2BheCXQyAJh6wWfIntMMwMNX3cNSwubxOsxS%2BCOSypOoFbM%2BdKXGhYZdLjUglXakffLefYuCXBlV0ggAtSoWVIIcYqA%2FP4Cz%2Fd3oO0BsWJ9EjY9YFKk1TPGvx6voMgLInOVSB%2FnmTtf%2ButiBRMCyUG3%2BtMUWhtPpoA1xqiq2thmwkq0AwBJK9U2osdSUryIyXA7K%2F5RVYYNXnIQvVSpmQ8281WbR%2Fs6hBOyxhTWbgQhYlP0c491Rf7eGYnlfzKFl92pJ6lDcST3Zwc9ql6d7OYPB75f%2BKazxOzuIe6paWbMXVIE3loD2h5x4WIePD6V96Swch7ir71mTmLHXEL1DBlzb6SbrX9QoTlOkefq2UqdMbfA%2Bi1sdWLcoVuH9ElkZHMoLXdAOlm6t7ChO3LM4k7Od4izWtQZnkLVVLFxqeBOhdQ36mJDPnSsggeyQ5yHy80%2BOOggdIqbSV%2FSh4Pn2PyhvgdTC9%2BbDGBjqkAT74r%2Fzp8TS2mqLzowBALjmIcNhuLgWlVV2lPlQW06hs2Lff%2BNgNstDnNkDPOcH8L%2BEWTTtVEdNvDFXDWQEaiUOpucde7DS2r9HQvY2zW9BWzE4Jd%2BSWctOUGjKEsZ0afgHNV9u2Fws5ILPp%2FJSQnIYmbgjiWnPiVSE2nVNSmhilmUHPJJndDK96OWlIRB%2BV9PKeZJcvoAuGwHODknAS6N70nAIQ&X-Amz-Signature=110554f9c21c90b88e5c0400e2d510eabf21e687421e3d2afb6fc79701580289&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

