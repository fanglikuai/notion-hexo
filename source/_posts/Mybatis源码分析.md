---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654FFNMPZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcowijKpCxXGn1inqZgfGJ%2BGKjMPk%2F2TuJADjB1anX7wIgdLul6ZpcoMtxH1f6vy3X3JpoheIVbDHWZqjrwl7PnEMqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBN7sWTscKE70L5A%2ByrcA46A3MAIuon8PIuXDOdzOl8gQ6KuxD8i5c0UeKCxP%2B2qWsvIleuBzO%2BwoMhNJOKr0u3p%2B7FRpoF40MuQf11FRjR1Lib2mtTQ2NJ9q6ubyNtxwW0ZeB6T9zY8BUW9cBLwMZCxFDL1h2N5ayLbi57ITnBbwDHplvxyP6muNURTilZLmGDemSgsdk0lqxR9jXZGsfZhRmHMeJJ25Y5vVH8kab2nFI39AoVlPb37S3it2VBwpkuYcHmen1s26TovrVAxsodhgyRZt941zwvPCeBwq%2BT5VWTEZJKg%2BgCocvm3jiNw%2Fvqp29eJ7oKNvI%2B4%2Bgb7zE1ARXDHs9AOF8JkjujDB6DKEwWGGZGqqDFcxTQ%2BpG4b47jjT0p63yGhWOInCX6Ki7ooSzQt0zG7T%2BUMrIKgHF7KEirIR59uAHyT2201ZWJT46XDHMquHgREj4I44%2FhaWfafi6FOdAZCYNM%2BaNomahWvmHWnd2iAjjkNZPnZdHjyEhfnMAX0HAweXrnCeGRnjU%2B%2F5i5wF73pdjiwTU17%2BUdB1aMUVtObGkYHY%2BkwOeed3YBM2HieE1H0El4%2FcWx3fvqgIW7MziGoRAIWfIAkoZ6OFQg9h86BesRBJU9pJcZuip50Fhx%2BVIQ4JHheMKKg4sgGOqUBierQjN01km92dsFSFwWOX49d0Yzb0aPYuiyRRdt9u%2FlNr8zoEGJTlH4ljC3UG6Xj9B1g4WSWGUDRCa2ODXsMmaQfXdOddsx5CN2uYptgI%2BixsJ1WJb6XDtH%2F5UlsXks0hkkTvIn6S9TtfjBkjoPAqj4DwkCk7nt3qdPzPsQXaEKm06%2Ff28q6qSs2%2BYGzyph9V9rWOgMv2vaTVFHjE6ij4kAikpJN&X-Amz-Signature=511927af4d006cb022390eb0a8ea6a87535660561d1411a5dc85e11f253f6373&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

