---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JKODN25%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBn%2FYEqPVTNScM7nL1Oty0BfmdxQxEzLQd47MTrsq2HzAiEA44GcTeP79QmkOBBHdFrxVL4yZfmXssNXvKJ1Ni51W74q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDGUJlM13MfwfoDzU1SrcA3rVo1CedfQlUh%2BKgGH9VkFXx32xWt7T0p72BB1k5wUgiwHuVsLmYgl3PPYgmyy77vH6%2F7O6IrNGbz8KAmAk03tGWTxxFnCAFY7oe%2FIeEYK3LDEuzTHb425FauDKIzqvHlDS2iv4n8e9y2w7C35XumMa8%2FnjKEsw5Dxf%2BQi5GwSKR6s9KOJkOqvlxHErf30HXPJzRvGtyU1xjNtC3LXvx%2BAmChyjoOofar5DaYFnrTad1k7HcMt3qDThIasMy803G%2FM%2FnYOl8G6cyp%2BTObQsD32wk30sNteT4Iajabr2cBHqgQqnLz%2BtlLWBYWj21EyNX6hDCAtT94nvuh3ikDliam2%2BD%2FAFJUzGtqk%2FO5ghLUG4fkc%2FNh%2B5m49QdTY99rmtDQrHeqMQe%2BwSLYgp0%2F5ojQwPfTVfXpwmDhnqm8qaAZDRR8j7m%2FO9i2neJEzrLLC9iHq3yivWX4mYM%2FvvZDnwKdaNvqyVRUIUh1haUy0nTWrVb8HsrRI1uZWO9XSJU0tDgezm5q%2FGseON%2BlWSMa2zg3w3H4oG3tfK4wMjf8XSDGOYV30v39aLh2hJMCOSxQCnKVHu36fzZyeIn7Hr9mHd3NQTxWB3twR9%2Fwcb2nU%2Faj5eJgA07VMcTMAP6SehMLb5zsYGOqUBMeK%2B5w3O%2BEdhM5ZI1IlWTStYlSxx0npxZ3dQepGZpggicS2K3H7hea8Myi4xFGxxIfMr3QoaVKtzKWUy3YriK0kDEZXeganBLpQzj3Nv25E9o2KxlhlMdptBKSa1OCtwy8vNyh9zHIRyqAX1h3a3Q%2F%2BxSZ9FB268hjfkI%2Fn6TXLVRKGuki8MUuYbVBPyVlKpyJSrdYVp%2B992Ma5pqvKDRvbJpfh7&X-Amz-Signature=bae50ab0eabacee8547299a485d6d02a01cbebf2bcc17dc46ec8c9f122ab880d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

