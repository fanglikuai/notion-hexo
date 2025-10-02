---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMVT3CQP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZRCS2QcCibxM%2FM8TckEXgkfis%2BGOZrBTAYlWHuAiypgIgZ%2FYja4CiMTzzTBD1ncckbXairHo3ETCa8uqKI8nu%2FWUq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDI4t%2B%2BUaQQ3ZrtuzFCrcA0oqY56ZpJFzjWtQqdtgxEuCwSXEvoiJLZqlh%2BUqCe4V84CcNjoCPKI%2FYvkqtO5J6EaXiMJGFEB%2B6gCSdMkw90djCbSVlHS%2BdC4TnDUt5FYDpGhWNxvRwx9FHyraLmNdnquab1yH4LbOBxuFRqoW%2FjgGFW6LCJzbUI%2FYCPHfmgrmthTK%2BQhuRM3vHA0m6WX1uwH%2FpGoZQ5%2BI4tTOGDc40d%2FNIkcXTay%2Bx5MOYfhbBSSMK2OfQcBrJrVwqydibVKse6LBh9NYzj50mzscS0yuOnVU%2F%2F1MD8hNOLDV6%2BbhU7UuxqIu0UTQeW09Ji6z3ctV34VA8Bw6BWffEsJsGFoUqQhW%2FQT13wU23oEN6vu%2FF8EP26YK66a4G03Oz2vup7aHcpVNkSQvWHgkFaLWMYTG%2FJuzJ4sD8C1ZYlwwTOAtwGu3mALHidAW546zR1cDPidNI7AxyzDJCuIF%2BKBdCuaZXqDky8q%2BrjzDdQB7sFnzKPPhGJdUVZI20gYFdu7zlLfoprjY6%2B1MV47YW3fKfix4HV3RC7EwJRj93RPMGRSXH1%2B1T5G%2F3rhB0p7QiF48coYyEzhnIa3AMIN337vCcydyz9DbdQhEy9FugQCktqSEk6gRDis8gyieKBKeTjH%2FMMnz%2BsYGOqUBvvnzByWReODJ%2FV6PsDv6VP%2FnrLuVp2Z4VCRq%2Fq1sEVeq%2FBFkqH6lQpUtdrrweA%2FsF%2FHYxIynn4NYamcCAOm%2BrNeWN%2FF4zHDCc%2F3383DHUm%2F%2B2M%2BglEQ5oty9E8KAMfxQHgB6by0XiZIhjWd3C5Z%2BYd%2FQL2pUyZMNg%2B%2B%2FvRsZ%2FFRkBy2ObqwnHkE9uC2w5Sp3AFCZUNUQFsbbwgl7KkVVS1aiI%2FHZ&X-Amz-Signature=d897f6c4e8636c8966b764e289bbf88d28493a89a41f0aa647eef838730e6bb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

