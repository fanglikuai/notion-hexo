---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LUAJDGJ%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3Ry7BXGu%2BvgvLBd6zp7SiRxsmScw2t8AesT3%2F%2BGeQ7AiEAkJkazj5B3w5MJ2QIYtIZtjfDThQXBPXYPcHOlML8go4qiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL5YtwksG39KUI59SyrcA1RW85TzCNUcuORAq1HOgLb%2Fcu%2Bp9D01DedU9kdS%2BJwxTOQctSPK9nYhqj4UIaGLUIg619%2Fuk1NSrGst9yQWPONLlxGxesthg5kltwBg09SYK7ZQnmxl%2Bcm55S2zLnNGWegnWGCCfLb4aOAYYBx%2FjvUcf2nAZIw3rhNEeQWiPjt67C6hjwqaYZyH7mNOhQHFIT1s34SF51jwrxBhKDO9XRirv7qclRULMyGAz8P%2FKLqkb7sedgmb1%2FduaDSjB0h6ByAC6dlsHqfKW65f48aU75kIsH6mM%2FAoUi5P%2BXNOoD%2BKaycgRutuCxSZv8krdjdEcfwyjLXwBMSxubYPkjse%2Br8OJ2%2FphW27h%2BSD%2FQ%2FPqJjOK5UwRuDyDLffDDeHME8ZKdv6WzR6FATvS7aA2Eael9pNxAZBhjiTduqb3Nf%2FDlym9e4WFQ%2B0K5Ft0Ht%2BXt9dUD7SRk6Cbg3Ok%2B3WXgxJF%2FlakvqW9sYTaWNa%2BOmNESNAibPyZt2YhhhPIbI3TansON82pNwlLA5HVlX2mF%2Fvxo6c4bhoK9H8pSIp58KcvSezW4azqZrU9yrXK2WwCLwSAWR5VCoqbE2%2FkvoFv6UYvLYiN1rAGu0UL%2FzZ2%2F8s39cl3kcKYxQNRoywbklyMNOqrMgGOqUBYYcqnpDMvz5AZykFnTuINt2xgYpPoDiVrQbFf6wzr8Z23vh3gGQyL6YHfYHRaaNiDYjT%2FMdD%2FeZxZ1JR0qJEDIEHfCwAMMxB8Yrh7Xfe5H38avMJ%2FensyQPCk8iUuNiMpss7KNTV2U2QmW1ayCdwIhf3ikZq5AytYWO3vIRHOO%2F0qVX1X3hY7D119Xj4gPiJRztb8akch49598rsqhlbpty0fduA&X-Amz-Signature=a591db07f8a8ab1467a49e98c12a95504465020d47351343403ae70ef91fd9c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

