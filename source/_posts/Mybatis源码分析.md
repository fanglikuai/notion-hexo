---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XDKPHLV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCNBwofgP%2BxnSYF6EDd%2BXik%2Fhyj0IbpL7w8f6tHpdghyQIgANDD08IxOrcvwRkoH89njJn6kwQl2WN%2FQyegYLaHsUsqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK9csSdt3AbJIgBKKyrcA%2F30MDKl5Ej3z80Q6MjzxwhCr4lZEz0%2BHmnoJ3cDF9LDt8ZlFe0xt9mEtB6zL00h7Sp3xm2%2ByZjk7SNnNpuwZkfDekthBhiNN2l22M6sv4ewZwEm9TuHf9x%2BHKzbOVndXxeJbgzzson14jr%2FG2x5l8%2BXgVE838M9CPH%2Ba4e9KvoS%2Bm3ryRh3pKm3EpCiTYRQV5e3iEqZ1XxnXZTUgmysWHpWKCTDqAv2mO5w4wuhbfEjKPyiXJrKH7aHnvvKWXUuaW0m1%2F0iozeC%2FF4fPtPm9YO3ITJIzLepjTHBVnpmeKuShH%2FxiLgimDeO%2F8koUyjQK4KDrYB%2BH2NkLK%2BpSQeYki7jkhAuAiSg5COQ7kmeazmSlob5ok1rs9DmSxvaOOzFMEKTUyy4ae9VCrnVI7oQ62HLAwL5sO%2BIFJKtAkxir9TjQ81xoQW2QACZDpN%2F8YZ9eq5bfiljJqxpFKNqqn50k0hSfnmUOfmRB2HSxHyXE86nDcg7TkssGl3N9Zq8FiusdHXNe3kOTCkXdsXJNK6rCR7KXvGdyUO6JEFQqqTmkyPxvnnH0IFRS8tB8XWmz%2FyLBNRoDJ9xO7YQIbiaFjFrj%2Fdc2tDGUt%2FI72oGOG3WueqLP0BzHKMiaD8BkKnQMN7nx8cGOqUBCravNtxo%2FqLdFkcBA9nhTpstRWVEu2d98yRwlIf6HBM9%2BTtn9i7ApB6QkuFdHyP%2B2RMGFgwwFqpVWvfW3e%2BRPCkVjWEBi0IGMs0tyAXO%2BzggIQ%2FB1oO1lvN%2FixInHvhwvimYbbvpHJOJJf5CZdbg9CpALjUnLDjDvcamLP6sQ%2BwyoTx3p%2F0DFoS99zn9udNTliX0ZwPp2i47FQyx2WSW6qsX%2Fzir&X-Amz-Signature=118e14d2d79a71d6e6d9c4fabe843f0a5ce8d8b2a68f8809e39aa599ec4234a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

