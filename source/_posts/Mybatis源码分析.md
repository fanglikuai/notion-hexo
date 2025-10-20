---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKDPVZWX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIA2dl%2FR4fd50eexECqcYW1jU9sNNlunq7dLvA3Qoczf3AiB5gA5CPzLb67Uqd8XougSbq28kdn%2B86UFlce%2BPplPNYCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqHWJ8wroxg8%2Fm3iqKtwDHoQsfs95dLQyPfaVR8%2BWTQCjM6fVdk7YP0hvEduC7iykK58xuXq81iGSQDoQnLvg8%2FartrMJgSqLWkWj9JJR423h17zquY2n7vGUz6nBFqXgIiumR1fk2S35qFsJZET%2FhxDOGr9lpcaDNuugm9u%2FchaBjLUUsgODG%2Bs0z42bRit3cFd%2BWaSLS%2B%2FRTDQGFf1f7xvXfnGM5lZAFlAbfhGCaHHCZhf6rPleLE9TqSZK7XhSUFYl3ZKtYUYviNkT9nlAHVBSLLHnCm%2FgyY34YHEjhdvQ2bSpgr869FZqhxRnjj0CRlNMaYkXjPZK2%2B6t7WZ0y54WY9WCgm9I7Fi3jLN9u6YcbJKuihIK3LijtjmETtqGTTJOg3cf9EezJerLvqwuj68CDk%2FsIvLNwtDU%2FXdrIvGbF0jmPQ8qaCSU9swCs2jMcCmVXyGqEz2o%2B6Skw9BmXXedRgoEhgnEWlxq23LumK%2BZbK6XpuuehAr2235C%2Fy8l2KUJwtBZSTdLB0BBEvXQfzEzaDFCnj7u3%2FYC8yLzN6vCL7wvbsrLGiKcJe50468mo%2Fp5uLGNlf4ogpcvvrSh%2BblFXVWdE0stupECfGAHub%2FqOXQgDJ2kYC%2BmqN2OhaRmlFAkx2av%2BE17hPkwy%2FrVxwY6pgEQUl%2F7tSwLY3EYeTkMQlHIgpIsFgVi7Qkv%2FmX1PCsuSuqUpfR2vnWV0HVdF6%2FCetgqI4pH2qGqRd%2BXR4AB1IP1ajwyRGO6GQlbPUkD1XeuIedw4lj2WPdr87gILJnsLa5NAbx%2FxSrYr9xMxVClTQqVX4%2BpGE8Fvb4090hiwHFZMMZBp3cxnfuxE%2F0BcAfZ%2BBNjYLI24OqBmcSW4I9%2FDwLhICboSLkV&X-Amz-Signature=cdf8591a4f71d180b93e1bd30e7a8331a04de98c5e5046469403565d9c62dfc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

