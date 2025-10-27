---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCXT32D%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcyWEh9MWVLOnRPDrM9UU%2BhY%2BAY%2BiM08t9e05KTEL1GQIgGM%2F%2BHut8OjDZf4R1dtBsxzS0a3CO32R26%2BIw6znfY6QqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOeapAQs0y0vZtOcoSrcA3id8eIrH4Q3y9tI9t8LffEdyKin7lJfUMTRmaLKxQridvWdh9WDQo2VqYyIDICsdQU1hM%2F8MIWJ%2BB%2FiTf81gPpncTo%2BJQ431ieQAAQ7VfbY4IjkJ6%2BQG8CLuKejGufP6rEe34Bwd76qMVnaNXoW4IbW9heKrk6tiWI0Vu2QE%2FQGxJxFG8VyWSjwz17Xu9kbD%2FiI%2FENmfaWsy5g9H9lX%2Fq%2FdXsv0di2CE4yzhwoPoxQNZ0S38ZvGr1vD%2Fet0uS8nymwMzZdMj28V34ZhjALtud%2B1DHneNK%2F%2BspCF0yHI5uMZulwRrtJpiTVzLgsSqUEVQDEpBGVIsBZfFZ1BcvuJAtoW2AnogKIDrR8hsHZjkSri9YOMlAhSDB4%2FYDJ%2Bqkl0o7Gksg6cEObTxbzOxJgm4H4Dn3xoHcd8edgr0sbnFyxkUfUcZXSdG3MT7F50E7XFPoJtyW193iiUfjyzMlm8Z80e%2FL8SgIE2PqTKcqXtiC%2FQOiqw%2FBuTY5vXI19pfEFqfBUxeCtI8KTaC7c0Jnslktmx3Sb2058h3BpEM1FNAaHq1mQfwJSgtc%2BvuP7dS6%2BB3crO%2FeRupL0b256txQWHvi7E78N8jhz3sZmasNsA2LYd5JhCLGULLM%2BnIRQNMPuR%2FMcGOqUBbiQAhcy2UEPF6woHoLe6Ro5xF3bh%2FXs%2BS9YSZpXche9T2ET2rjzav5rG6BlN0nLcMfIgOfiQoMYzUXu85luofYKGuRTWXpX%2FuG1JbCSAAN2fSDNTFl6vpIg%2BGLkb%2Ff5Mq40lz73cQUS8c6ahmH4s5qm3PXnmw7bjDX8oywv9giiyzu9OSDI5VUb9jClz2Ubm%2BFc6qemmOY3yWxx%2F9KDA1cg%2BTjuq&X-Amz-Signature=8373ae21784898e594c8b0f3dc19b8e66b6a85b5ec82ab80144ad6eba2ebacc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

