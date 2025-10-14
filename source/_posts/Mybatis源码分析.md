---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUQX6LS2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvK%2FQJkdkyRnjCmalLU2UXwgnmw4DZvUGxAJ3%2Bxwc44wIgdYOanPIxUppgoue13dPK%2BLn8TKX22KYse0GrhcM%2B91Qq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDNHTfIAK2tORVpHDHircA4laScqPOVFbjgc5oDjYfgngTwQ0%2BeIUMJ%2BdA%2BZUCTunNndDoyzg2FLcDLyDNLUT%2BSvxRS943KP46Fk0uZ2cThKKbF%2FHV7yhTZxAjgxjzGqwx5QgFkuPCEKbc3ZJuYC4edxEeyj%2FJsEQEO3mR%2BeABnDu%2FSldDC5Eg%2F6XeW4sqqFlNF9DK3jCKB4uVsPa4PA5u87J4pVkW3jqJ0StiVxLsA9tH1egj6OAG52MXx%2FpS84B5liKqQVJ7JfYuQ0%2BFBFvp2ysIytjrnW7%2FHMZJRvjdG0wUdRkzdojv34FbMsbByuUsR5xv64Ys3DjG2O3DmdNYNfEaJhr6fF5lffPNc4cS6e%2BRkqfnLuA1EMwCnDnMyNdP%2FfbtFf%2FXPtZToPx7%2BX5en4AKjMlyWUtqOMViv4nIkA3rLXqbqhKH0q5dAAkhBw%2BkjdKPxnloVJgbVmESw9ahtQZ%2BQfFhzoA7Viun3WSpg5O3AMfgHxaeHQFrVVLeTv1y9rWyLm65OgMtAezPpfb7%2BAZ9%2BpFa4vYWggjj65DfSPpZFCrBkxcSI8Pbj9cccHobWMr%2FFrgrRPZmklApyCe1C5xcXb5C8yKnpinD%2BvpfUEVC5BZTys6D%2FdD9NB9wdOKLh9V41QB%2BSYbKCXtMOeAuMcGOqUBzdEdVQAhvqgNbbpN%2Fd4PR0SlN%2FdThEalR6Pe7ieWUnEd%2F3NJhg4rWsFuoBSKI5WTgeWtk5KBVIDPzjKGtmHV%2F5y8oozVdi2U52KoEW1CsIcNjU00ArQbw%2BOdfjWBi4G7dNB9L%2FlBI8NwinQQsXQIhUxmyuJso3o6m8cTUYvZCHjn9rIURJEwGbOZKPMgZ9%2BF7phN4dcAaEfdZALpYwTu8N9U0mFx&X-Amz-Signature=e770fab2efa23175bd058cf333b0837099090fa91578d8455b13d48362b22829&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

