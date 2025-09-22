---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSQVCEJS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG0V%2FGsqPD8MpZEd6lV7LLxsg5eUQyFalQb7Fx6ooMbjAiEAi24X9RjwZDA%2F98P0zdL1sH5ySdHoSsZVzwKLOl%2FQhvkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEnubqTZ2K7ywRIXBircA0atExx2rVlfna8QmzfJyREv7V%2BV7J0FzTK%2FyQOuLGuPFfA5eXyrlS5XTaYKo%2FzURfnZY86AIOKzPZUwVh8ExhtwRSx1YyYxNU4dndzQOy1FfU%2FrmL%2BRK43VZoxtFIb0uCc67bTp0v4%2BitLRfKj3mUCSdMX0NUlqroRwxTWP3VqMx%2FsbIaPbnn9Qcvr3djyXkXH3WOeNwHk74HCMuXB6MF1MpFQGK9O9c3blX9v8FFTEZVSGGRgLSwIPGFxGGJQ9hFNjGcwK01KaZY4wVsqyePvDBUVy%2B52D18%2FSzBsljPk57CZij3z4nkG84viS9QvmUIw9p4bGDRhJCmjYjWyMlBF5QUIYkHFQk6GCVvD%2B2sSV%2Fcdfx04s%2B8HdRC9iR0Ya%2FcoZQo2LLcNWKldrBgvsY8PZuZNCcJap8olsz6yPVIAVlwRgfj14bMsn9Bd9VMPBzznhSF96vSyqY04%2BVyy37bsPlwSNVnqia2315fi%2Bak849o66bLY4oYqekePJ8C9J7N6kEVQOeBVlPIr6y5PBtenURM4bNmwEy%2FRTLDWga%2BhJuvRKFf5RoeBRCxa5DtflE3aBCV0VjA0MND%2F7CRKepX3SxCImG%2FKje0DU8C9gdthJvC9tD9eW0Kqq3qlQMIbfxsYGOqUBbBI4J6RWcsYj%2F18TKKQ3MktO78xohInvDCMXjTsSwNeYMnwEJaPCF5oOnvOQy8pHD4kFfUyDJC%2BuhqIyWmeU38BKxOVmdhR8CIWIcEf%2FACAG5nku3rayT4gWqeHmSd11BF%2Fll4KlElk6o6TDhefHmIWAmoacOF%2FCEaYADGt4LgiHu0%2B%2FgfzwDJclK277X6CYpGl4QnVcfpmtnMFy4l5d%2B2u%2Bm7D%2B&X-Amz-Signature=0528da43c7a88b2b294c638b9a0796f87bab6702a4d18ec3120198f08bfc86bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

