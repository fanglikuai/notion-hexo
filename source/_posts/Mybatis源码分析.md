---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFI2FRNY%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHeMCgyCzu4KeXcGEtA5kT6mkqN%2BXcDxkWPs9sQAI7YvAiEA9t5z1oFQjNzuecX%2B4JhY4SmbRk26nuBYKfFMc43SdvQq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDDrDaPi3DOLfSKGUDSrcA2NRigtB4oa%2FHy14I6Kd01uOtv1nrq6zHtj5ECqUAEVkGwBbYyMN73J2Z0pQV1AVLGqbuNckIzmQuRKeaWUPyNvleJCaurPPOECqKIYchU2Ftr6uH7NsIK1IZfs753aUuqZ6QKR1ieK1TUK%2B1SKOqpGz37Oq%2F0y17ooGJMirwoJGDBC8RLCf68VMfimYys2%2FoXP5OGdXWFjWDwrBKzOPt5auA7ZJtquS%2FqZxCNnw1CIxeB4hN%2Bn4lSkFBLVNjEogC8G9xpNB%2Br8%2BQyQo7M2%2FEFMqbtV7IGH5PjhPdSpTV%2BdVDCKjeEsODOsyxT8krmCPX%2BDnyxU1UjwDK2%2B2O1U7oz2HNoScY8eKrqb4lpdPQmlFzBtDUtccS%2B1Dpyzg9EnoQPInI8yAvQBTYZGyJs06BSAvTzg8loibzM8muKGyR847QfqCLjYZNEyNR9oO04rT8Kk8KNdwy04duL8%2FJmO4oLD4CQ7k0qaOcfNBfFcukCH1pgQq1PPtL8EQiPTbHA8ecGa7THw6gsbvxdjSn4MlqHgvbf%2BgRn4EZ7ggTL9KyIfUWO%2B3wCQ5MKi6LNx2eYKCtD8haSLbQgp3hkA2fB7KkbaL2JvRVlYV7wWJGuu1wEEheHhh5WFZI8x0UQkpMMfQ9cYGOqUB5KqDkH%2BpBTyYG93c0%2ByTRih6BWiPNpt8lRhpH%2F0IlqE5raplPp5a8Eh%2FI6GRCIz6PMnalAkMckQjjVOPQVztYdWUl2QoltjYYJyTPlXhIo0jaT2Qv4XYIJ%2Biv8Ps%2Fp1BUH2rHa9O4KM4yszLyJiosJoB9iWlHMyeMi29XbXyu7XV%2BPC%2FHZ0qX1nVNni%2BUnr8906w0D1yvbV2w4HIPjUY4UOtBFub&X-Amz-Signature=7d4fd8af7c6806d75dbdf4bb0540af049ffddc863210c69a003cd665b9a3a9d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

