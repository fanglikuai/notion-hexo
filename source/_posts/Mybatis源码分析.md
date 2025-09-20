---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RI6LYSRH%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC6K1Ro1vDSuygUoO7X5MyFnkBlGjC%2BiejLqDZkvgziGwIgZ1nt3CBc7hxq7XObQuRpUstLs%2FdR6joNdwlPDYpcmj4qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNkfgqIfbw4Y5SnghSrcAytzTlqVeC2aftb8Dlq4bchtrhgQMEPr7km20LMWinkhgu1SRE7XgUqNCEHSjZw9D5%2BRNSzieLYaSnqnrvMebTvvk3438UaT4Xlfo6QLGJcurXlMjXTAugCpimpfGpHWPA3%2BK716fDn0VkpwkG4sHQLiBUkvlWyJCKEnl%2BfaIO480z7JOLCHa%2B2oAxSin8Y4scnT6Ir%2BaNEoBbBRcbkoHjBrEOLo0SBs8L4FxU898vhoeFfUAwDJL012XzSOZh7YS6EvNgxiOsSuIzeEbtauQMlPQIlxuTxdDyB7vsWnJadKwyJuIwfzg3FXLbuvyUGA1wA5oSbLHaAxZakgxTPZ5OUx%2B93NgrpxOmuYs9Asn%2BbeseXfullSdjxhw%2FHx0mcVsexP1ph9NXqBZA8kncQC2Leybj9%2F1S57bboUurSVD9%2FJDz8NhiKHk5MlvYygby0gimur89z3OQZteW%2B9ishAqc2ju9o%2FPDOeW9QNSwMucf0kfD1TfSdUFcIcdC7sIcnfa9hfOmXOAGvFek%2FvmMw1RLvyUDlYhWDg9Ql67iP0f21TrdzVc0QWgDVGvXKIT4hN4hHGwOhGd2vWCD9H8PWgny5X6U%2Bf6h9xwQDfhocdEOZDpqk76D2ZHYNaROWuMK%2BkucYGOqUB%2FlafnS1aCRWsWrx18KShbgICG0ikB%2B7SkJilN5r6Otlz%2FJ%2BtsOFIk%2Fn76H0mtCpxGSar%2Fv10NdE8ptyF%2BHsoIXCEsr24nYxcCH2OPNUQq%2Bf351tJMSZB54jtcknOhMQHwXJE99EL38v2AGq3PG9YNyUvj0gt8HDansK2%2BzuL0iJSmbAOLLwJL2irYdsxw4QTiyciwgE7jD%2FS4d5hUOoogYG8S8cG&X-Amz-Signature=fccf7c0d2d5a31c4738640ce10d84ae5f16feea55af44aaa8fd604df6d31f53e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

