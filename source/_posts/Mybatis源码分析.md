---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HAGBH7H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpYciofUUsq3cklprJ68PHmhzn5Q%2FRIs3l%2FZ8drJloWAiBZfgabvuOIDx0OreVWlkzulaHQQeMij76TzYlES8Jh2Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMVzamFRnL0A334MNbKtwDVWs8NAGYRG3FUKMjEja8dPT6ArJqO9VsPh1re1CfzKA8SRuwhuJKbY4cxDMhSZkfikPGs%2FGtRPQH%2Ff9H9MxvRjjVNt9H57dFfSMWDlzFxQOi%2F2m43OByNoGigvxfX%2BlLRC32znRpgTRvOs7sJzPEydx8M%2Fijn%2BWUlHb8LiFZZF34Uls4Xa5KcBAfVXHjp3rQjRqqYvmXxbI8yawfGUrY5wwuPQzMKnzToRtQdqlImNt02s71yONWcrb69CDrymiYbazhRQIAIok4GBgZmJyRyvos3TJ7JN%2FXQvuDlPw7fkvz2GFIQbBd%2BX8mO%2Bs1A3sBZbvdLnyemxlADdb9gmwM7J6hwpEDWiyRRr7l6mlhQae5aH0tkDR98OD3FPRt4By0R2eT6%2BRw%2BzYpw1D9LNtzMlxK6Fn9evVRpl1uUjxCvxPpuIDgf8Zh5wlcqrDYFzMNIL7xa%2BqcyAExhlqTzwvr38rFgCdSd%2BqPtBML0h0JVMc8mxUz%2Bl3INDzMr%2BCw1u%2FPcK4FGoD7gQDJPdBiV4RKOvwZ1H5NAt3aNzV5re12Y6Pre2gEUNB9he%2FJJXfNrpTbmmLiD%2FnuhomwoWwRm2FOcrQIR82UeW%2FvoniL7HGkJW9UxArWH0iyORU73FMwhIKIxwY6pgFbfO2oFBvLXD%2Fv%2BWha5NPPZAj5UFzZEPSSBcPmKF37B4%2FxEyXn%2Bdb%2F9500YGlPoDIf%2BnfAlqNItQ8R5%2B%2FBpVqiL5u9R17hXHXGTkbb3aWwjcE%2Bwk%2B7P9P%2FKBugyxNFeCWISpnT6WnNLtZ7Fsuk6wNuS1Z9qZK1szCo141hJLzOrZCRa1TC9Mr6%2Be21ZuFvHRdmvHZbrZUiAk3bwRIfLo4w%2BHnFNfyx&X-Amz-Signature=2bd7ef6f36c4610a48eb60d0f2da4a2a294cb9c9786db46d3d473b360acd13ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

