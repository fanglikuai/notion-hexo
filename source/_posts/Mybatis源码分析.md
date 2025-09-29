---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGOOPBFB%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHA6RYoaYBNwk2g1uyu9S9jrN9EI5JscyE7nd%2FPno5jzAiAYtoD0nUjbLa4ABoOgkTyNz415ytJmf66nT%2ByFDc4h1yqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8IvKmjfSYVKRfyDhKtwDVObU%2BiFSlHGcpA6R4gzdn2bfDmuKlAf8HXMMppSGit1SnyUUeTILSnDInRq2w8y79gPNTts9cDVaH%2BCvxdne1kgWfXE1tKg8U6fHSomWvjGPc7XBqr1xnUFO2oigxGi3cdOzBA0hE0TkP1m8XmajL4USnMVVUCUAj2cNWPT%2FEkRDke4Ku3%2FtOtwDiwWRxvi8PisyL7RxSL6G9GjwpXS0Z%2FdDLPrKlW8t6r9ppFHCBwywe%2B5n95ijC1vckF7FEvxsRFXc5eTJ3pNl%2Bs1JsuWV4igHY0b3AKW9q7ePGaQNy%2F5PZqdzyzEiwMYcVKiu0bbd4bHzGeznHL1sZPSUGbFafvIgDmiiKDnpSbaEojvxkHb4vGNbPqbtQdHpj2zmmSOKpK2J5ZXa1iyn5%2FamX1Z4Zw3ZKsSQ7rA01ZDVr2lVnpfCfrZLz4uNdyHQ4iJMxShzvd8jSoQjL0I17NgI6fMiVrGxeiWWY4sVXjohwzWlCCyuMn4sJyPj2M5TFEOKr0Zs3zIsH2zE2%2Bcd%2FYrj%2BkuMF6uks3EfnnD7zC6nNxVzM2ewue10Dt3zkl58ZBE7PPuUcCZ8SpBNUob%2BzIEYHGo86zahM3cgiG44hPVoKU%2FwnU3FsZK5NooK7ppZzTQw0tTqxgY6pgGok1S4Oh%2F9Utod%2FC3Z%2FZFGNHxYYEyJ7XSTUO1jl5nRENNYRfrelWFE4cgsq1PCPYSrZlEbyZGi5FKBEIWK8PEMhjuhHbjiuukfbsMR%2B7563I%2BwhS8%2FYUnDLBrrzVxlJx2ZO6hckCIEsfsrKLwPldackKozlTYXomy%2BjW3IyFc92YpSySqSMZTzmPVafCWmn%2BmgQFaFE14GM8PNsCYriAHEmTWAtgAu&X-Amz-Signature=526ef371094a91136ff5edeca436a92da06d28f942e2239cd1c825dcc4844d24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

