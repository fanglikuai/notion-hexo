---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTEZU7V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIEFuJoTX%2FypmiXP1pwaqZTQ1HyYa40jHuz84GN4nnsY9AiEAqn87kikh6Mfm%2B4c7lxHQHe9UN2%2BDZ%2BP6t3tho7NwoDUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaI%2BvJY%2FJxQA2fcZSrcA9iguulaXp6eoAr2MLAkGT7ykCveosTUpmd8qsUGVOXoNX9BdGM%2FfJ%2BN5yF%2BYLFNx1gBFTbuHMkmnTUUXCuHh%2Fe5LJGCVUe86GXXSa0YrVhQQkkerWXjmZ4pTPD0XlAIcoTKjqM2YXB2Z4NXfGX7UTMBjEurF%2FRr3oMjrHxSMxYOZLWqyHmy2m73T8n7U%2Fs%2BerAO6oT4cs5j0oXIt5Se%2B41StLOQ4wqdj5EsZDrohGL4hsohVXu%2FiaOD0brL2%2FcLNpz4ksrOB5YLwCS5nJCJ3A4uxg9w5jBR6zHFLiEtuOOK4yZOF8vwyYfikD4JSdJAu5ne8L%2FdIt20KYPMkChTV6V3jirntADCTz5bauupxy9X5h9hzQulNADK7KFKjP3LX4xsUy8OJgP8dgw%2FMTI1%2FhHxQw%2BgqKOwciQOghbA%2FKyE8Z7S%2BmeBFGnZf8g69%2F5SzWVR%2Fug577pUPUxkwvAI%2Fd7LUgSVgu9LEKE2JLJqrqygt%2Bx1GLFmVwHIYuoeGeT4b0bVNh%2BXJzfu0FflRuhQYrXrY%2B6shCk8aDAAKbjJp4FLGy1rpJkFAXitVeRtqWsqXM2LADa1rq3VLm%2F4kyMn%2FIn4X5lOSmhy2Qsh1HfNDSrn2tFDui0RYQFCEtBhMPrUq8YGOqUBwDCA3q4yhj3CxIkXm%2BbgwROpBCliiKOMRKzB8bnCwJpVGuAMCXndn%2Fq3DyktA91zY2HBCXw5SlDcKGb88CXaKF1CpSCjKG9ATj9dR1CzQ%2F2chAmDtBUfAZtceHcFE%2BEMPGMX0VB0SWxNOfhgLXgGg8yB67B1PpQcSncnnlwznLPeO6TgqcmX3%2Bk42pYgesQW%2FgqVFiB14LqU0HubpyJXLo4UYNPP&X-Amz-Signature=cc84140a79e703bc58d31552ba8e1396d26ab4f5e146c9f40fb8ec26cfcd97e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

