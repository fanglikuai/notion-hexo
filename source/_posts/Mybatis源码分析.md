---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJYYOSDO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCICemnaAOBfldpppxwRa%2BWUhpYzXNUhjaRC1Cche5QsIKAiAJGkVFJRi%2BTotLV%2BZO2vQwPo0fJqHG8ES%2BxbzepZmBcyr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMH6uEGbKFo6%2F7uQA5KtwD59G%2F4rYNBccfi4o6Uvm8aK7nUufO%2BR9bFNs%2F2pBPM02EaSujmMoQUY3%2BA5hS0dyH3uKMxVshcxIqg9zaDKEb5HTZb%2BoNbK5UIhKpA%2FEWSXOQPYakK2a0SXkoGOgdPyPGQf4B%2FgDsKnWQU2CFj1APu5cYKNAOEkj24fCGW%2FBkdzJAjgLZ%2FD36lqYmBA0oMMxO2iunl7rSzHtSOWJwTmN6k6OvXKcRQ8YWegdKuCjFXALpQSN821XdtYgBgo%2BtXsgMbgtFoEOibFUfwT8n0EHoqG8TwPzXCheAjoC7k4l9E6DfwHHBlHuBJ8ntL8SvefIKfNltLfHtZB4FtvEc0Pb8z7m6z%2BApgvinjvhZ%2Fdof%2FZt3JkPZtEALainZYI8mbNoPrP6XR1m5JV4SfGTYg6GbX1cz769k93yxQ5JJlt30h%2FuiXRlDokXL2LEh6w%2FApbyzhvrARZrcieCV%2BRpj5D5fePF8LLMf%2FsK%2FCV62p%2BJZDsqiY66KHFGTNuSfB9D%2BvZovczACdgbQHmR3UP6XiYy8Ajy3UfkZN5xxyJj3QGDxOOGVYzarvX12UsPomc87oJUXRSsMSp3tXkxXkcGzo%2B43FBup3QQqZFgJ33ty3RVyTDCnFcpSau%2FZYW8fAyMw74GJyQY6pgHppQzmOxAden1PAwn7raoqChZZXCHHc1Un9%2FMl2G%2FDg04CNJU09x7445zpUo0%2FdcYEWAq3aqJ8aTb9e5XkpF1xHz81Gzr%2FeG4LCJ%2FxG1KnODmmQHLMa4OzMmYSFgzY93nHhEIMByE%2FupLk14IbQy89dz6kGGCAsRm3RV%2FfsgS8BnuQFbb8KoP0XsvwhBny1MbNyvju3%2BaIigOLvD%2Fhj6uxvkNnUwBu&X-Amz-Signature=5c40fdf2303ce394c4c889ed927f473b4dfaa4daf40b4eccc76b1de3ead34bf6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

