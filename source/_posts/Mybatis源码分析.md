---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDVOOSOF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGyrTb9ZFLfK6RcpVePvjnUeLw0iX9pWYYF%2BqvyrTqstAiA4ux8jihGVNLIFIgCbrjWA8IXR5CbmSShtG263HuhY9yqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrCjWV%2B15PVM1rBmCKtwDg%2F1P1Bf7p57mfaE9Dm2DQy9lMex8qS1rtoIHSuNgeI2YkgX4NoEjTNTBI1CqKr50bWwgotC04piRAsX1c58%2F7c3uNHUnotLgoCBOjXVoKi0orziQNvX3sJhaILjlGThD%2B1kGaPh8e7xok4nhp15F5kZ%2Bkf7aKbkBzZTIiwE6kPOezSX7qoCFjd2BQLNfDI%2B5PwwFPbX%2F%2Bu8FdvN9i33u5zuk8hNmWWgYScpd9gmpzF5tz3b%2FLRzyxLR5K18w6xd%2BCjFh%2FQ880bqlaP%2FNpvFU1Lr0dFj%2FjRrDL3bBc0%2F%2FPEatK%2F2jCSR1z3tJuLXIOyxfSvy0no3bpXjaFtKm92RMfFnvWCE6rdCpYXneZnLfuYaC%2BdI39FHujD%2BTDNLPWzQxFdo9OXOhHRMHbXrBci4anFY8in30B7NjRJft%2FzTueK2pPoHqJI04a7jtiBe%2FZNKI7E0UTPifY%2FkhNOtVs0vW5f3cN%2FQ%2BTAX65BSXZcdf%2Faff8iH0U0BC8JDpCilBX35RzH60cx8Nr%2Fh5R4Gdv1jSbb3cly44tV6VKKxovDi59vfcJzCFWyiLWaw7fMOIh5pt%2BPGy5IGgFqhEzPFEMegbZPdrs%2BsxFHz%2BVx7iZBBE5cCojHOiSA%2BdXBrwdCMwr6fgxgY6pgEg7cXJMZtmLxTT0cn9ox0l8HW6CNbSGbT1sIyDntHehz7OY6blQHEcbWBBmw%2FxkX4p5Sj3drEUurA5F4vYvzwpYD14foR5GkqMO0w%2FDnpYJGzPRT6LoXFW1NdMshuxMM94Y2Oxx1IleYP2h3zDiUY2aOr9EFioQdsWgPhMpOrejTlI575ZIN7xYPABH8hI4KSkEwi%2FF1aBpcHqfvU7v9SEub6qQPdy&X-Amz-Signature=d434fd464eed6f36edb4960787169b9101c7398dd4e2966a15cc385aaeea398e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

