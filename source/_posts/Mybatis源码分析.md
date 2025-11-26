---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWLNDOQW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDhr%2FihyR9V%2BAWI%2FrsPBPL1Nro0HRQZKx8dpVQK%2B5AqfAiEAmZGa0ZgQu04AvhQVC7vIN4mVR8Lpyf%2BDPHK25YjQw%2FYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN7lTFOV%2FqsfBPyySSrcA6WTHhEtv2GtBLT74AEJbK%2BopF65TwJac53pn5tP%2F1YV2OCJ0BbEaiq%2BIdfq3SDeugHywZ9HMWshQw8OCo1JNsUFIM1C6o%2BL8RdGwl7wwU8HHUsrCrL7B1VJfbKzH9pPhdtKDR5ED3DJgaSRWKHv5WfOBCRbMnhVdmQd901hWo668yBu3QRE1zJmdiPMt3CcZuxy5deaPRnAGAZzyX9fMWokzz%2B%2FigJkETHJUjmHAfHsX0qsQlgCoMMQU1980yWqv4UKRGJyCeFWHdzzwgocztN6O1fAy91%2BHnK7JL%2BtsCxgUmC2yF7KSdZrssxD%2BJcpc6RbUfjCdF8npebCfGGXmmbypeJ%2FYNbkqbfbMWchEFRSu4yXaZwSQi23NVhmBoh9dFNX8yyV2LtEhHnuyCgSei2oCWL%2F%2BsKtoD0CkgP2iTh5Sn3nlMcsPREU1G9o5jxi4%2FxChv6KnSEF8onOiOYQJrJyNk1gk4tTfuEoGu8268cHrrGmw0zMM7T%2BwtIktQd10sueXkqe3rD7aBSgr6qmYiqUmN9YQgSY4a6FZ505jzm50bBYdw%2FtEk9UPXlq2%2BY7XIrS3Gb1w90s6Sl4GOy7P%2BwxRhcRoVXAHOhE3qrfHZ8IZBLblwiWq4ld6ktoML7HnMkGOqUBNSidkjR3YV8weS3%2BtY4Dyk86lFVtlZZFDdEtXhWlzclOg3IWVWuHhclOdRVjJu214SCkavC9rrw5u1zW9zptyXoJdMmPL3jGRfo2aXvBhvUqFjhnMj7YaPGK7eECSb6JNc2RhjianrZuWGyRFBOtD4%2FGWhqAzwS%2FblM%2FePl5Br7v2jA7nqTwOcIOH4X%2BLRWx4HNnCdPC54mZKgctXM4Ke8ze7qa%2F&X-Amz-Signature=ccdd675701aec739ac2bbf6c18cbf9ca39fba949c0d5d342fc8a3c62041a6218&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

