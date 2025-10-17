---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633O5X765%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDmu3lP7QrLWYopeC8cI2JHUPUQ%2Fx%2Ffk8mOfjHL7KUjbQIgZ00JiPgnSxVqoOZ0MZJ7h6vgBWW35lP23kLwsDh2ZQ4qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAiIyf3o00xve2GrlSrcA3vONDuXvKWNmykNqK%2BeEjw5FffHyqM3357aHSRN2sGTRGCEAnRV%2BrZEu6JJcBTe%2Bf34axLeQ6ZkC84SmGX8YLchakgtxeU29Ii02ic2F6n0TGogiY0k2HFDFnw5Z02X%2F2x6ioXCfMAaPBSlR28SJCQPb9aHb5FG%2ByczByLd05xk7eMzTKghR%2FqFVuTqyGVmReKxZ2peyUcb5A40bL70wXkNL2%2BFPjxekk%2Bk7AFCJcKSZwlwL24wLzKY%2FQyc9nvxGr4KSsfkWmKJatNr%2Bct8m8XtpvpVudYy4LWeL9Pt9Qu8tiJgokEX3BPF3ylGk%2FQQTupomsdRvtjsozJTWSchzwlS9b2%2BWAGhXwUZK%2FQ6gzu9%2F4vvYNaYF6a8Iiu1yCXxIpmJrCuVlQDIhBpyTA%2FdB6ilaT1o5dAt0OfHczIH6QQvnDffDtyWSSK9BWxGYYY%2BJO1Z9vXw0wUq036g5%2FJS2K6HAsaMmEtRKhh7GFQk97YB7z4HgEtXML3vTffglI6vBt5fhvxuZAxSE6cjuvK%2Fu5m3nlZhDmDOQMrHXGHYHmR%2BywdPV%2BArAJIHhR4%2BXWWVuQB8wIEVXVf0z5C%2B2LM5kHRK56mJXgHxbP5jpDGyPynzUE3lihmrAiKj2k4pMPOEyccGOqUBu0toGS2rkH8BkpDOYvttJIpx%2FrtPGN%2FYoPy15WvAtnMF19AB%2BjecspUbtjQqgkn6e54xZ3%2BkRPY4KtXy5PXJfjEjgThwbKH8zcz8m7dXS4oC%2FFbsbCu2fJCgn6zzwsLVmt37nRT125y9dUFBm4leAuHp1EB%2FJftf3sT41tA8mffBqjETN7c%2FIYfmZfB7%2FYZig0iiV%2FvcbSD%2BEdcUeQCpW2c907z1&X-Amz-Signature=8ac235628f74561dc5880bcf134f461c108aa2eef358c2b9221c5313facb008b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

