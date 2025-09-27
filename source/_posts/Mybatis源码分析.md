---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652BJAMLC%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIDmuPn1sM3SOAb%2FQLPBH9CSC09hBl9xIUQRHt3GzGlMwAiEAijIOXxA1RI1cEkMCxPSW7Pf0x7bOJ%2FMRsNj2pee8cO4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETe%2Fvzr62RsIk2YzSrcAyUegv3ujh67lborUQDEXYOCKyUGASCAaa50wTnWgIachFSFQD3nnL2Mf4jwzPlVe6NugyhGUwfesrZGqUV90U%2BYANSzjKiggq2Bg14Lrdhhz8hvXulBg3hMqSQBRnh3KvK5iwS51nzEeEU3Gr2p7FKWAtqwUzpLeK%2FD8SLTKxm%2FK10R0hmrDvZENMutzLvntqr8LLMS6NyxxsGl8HscI1pC9EE4kQaxAu14My3xKs9kofWLJ8NjiqQPWgAxOAXaaf649URZM1n%2BkSZx3%2FafECo4N%2Bzutg2Y38YgmX0h0a4s%2FDgQjDGxsk7FqdTQlC855i1QEZiA7dOm7L3hB4%2FN6FOG6A1vwqH6oAfhMrnZRAIjgql6%2Bl3htm5KOHa2VnWgb6PqDt7Mws8ul0PkU85MtJLcgQEcbLaI0AdO427pN0KcOiAIk9G9qB2BTfQpwtpFyxvl1HQLkrkm%2BYwrFqcCil%2FYQZLx4x%2FHnf%2BWxQlW6ZWxk%2BpIjOM2lyMmFkzB19NtVaGZ3xZPKK6EXzeKNP7LTdNzZImHbONQ%2Bbg2o8ALR9w916SlJbny5R7YGpq3b1G1uM96fKf1RdOLFFbzRxh5y1gIKrzuUK2pwQKdfsnEdEUf5ypAu19HXXOhS83kMOXc3cYGOqUBpilb0eoCflWTbxwtq4Rbx4DylMPfA5T14GgGg1x7CkXcpJY5UZsJfOn2KKjMcvcFeCbRU9aIB7JaPuw7Kzf0SXISJLV8DUYblVc5M%2BG%2Ffr8wp%2BgqjPVFuoRJJM5WJYKyh8R3535FdpaGLS61JCFbyg28T2gxJ%2BtMuqgfB44Y7ucJY2FbHKJ2ihMwdsPTiuYQoPshSoFyfed6AxdLGgsNXD5ZMoDk&X-Amz-Signature=baa125f25fe5c569254c94a0a9b5e6ee678596430acada52f40309d5c51d3a3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

