---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4TZE6XR%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T050113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCICTRB7Dr8Ns8txzPUMvJttQ9ZTEkvX5cJrr2kROtlGl8AiEAzdsRU1LQbDIAWcS63qE%2BfCZCSZE1Tm2oOswYrozsthYqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOtlpy4184MhVgZ0yrcA4yuhv8en6MEnrY5kTJzHsp8lbdwZAwr%2FDiBjN%2Bd86fui1yP5AHM%2Bw5XjgnNb289k32lgQZ1DcQZ8rCMcqcHb2z%2FCaF56VTagTxLdrg85sg%2FkRS9oTeEXLheZAr3S8UzyuK9uYwAoR7dp76h20mgvgoRSbUJ6Iv5BwlLvIyEhdK42BaIqbhCJrM5ROj7HXOPZ9pVbP8rD8pKigSbzJIi6RYTZDcXC578OS0%2BBZx07kwZBk0ayOXUADrrwPjBpizg1QEAfX%2FFcn5nQTCuKEzIBSFktKnXd1KUQa8yrK0eB2gkJbkXRj9S84GwpWVzZ3f1TS6935T8X9Z%2BhUZkzYBcpG3diqeoxD%2BBmCH8Y2ogQXnfsATL50irTtluMmu5JlCW26%2BLbbsPnRnUTY4sOUi3B5UopLDTiayFsrr%2BPZkqO8YkjN3wpQ3%2BwZvY%2FGcQVZZot41kXLaXAJB4nATZdxiXIAaYssc81i0JMTMr3jRbl%2BjqApBDWmVSwbCZijamOXnOrg%2BkNCYaavhKw5muBdvwNZp%2FY7EXxPEffjotVXbaKVxNkls70Reu3Ba1%2BxnKFKenI221RVKXRiMTx8TLwdtFZKGWGVuO44ZVCrv7KPpn5JGoPKxZfdZqts%2B2Kk6TMPC1s8YGOqUBX1IdPBO0hYUKBVBI9KfPJ%2B8KV4oI%2F2lKE2LITe3ihGAzVb1777PZi0z51dOZokaYPl2B4ogPw12ilGp5pmar9vYc%2B7WupSQTYzxbhskaWIoOfTR3hSBjDqMcDbzdeccyLTBkqdBRxJ618ZRCUEEEdOsfzqXy58j%2BLHfHZ05EbCf35GS8Q5RG4kJUAFvLSYBv23JC5OKOkcLShprUezsNKBGbt536&X-Amz-Signature=24ce30b51e7dd9ddac9a25327deb5f1d472c594cb8e2e5bd52764c9b5ec245ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

