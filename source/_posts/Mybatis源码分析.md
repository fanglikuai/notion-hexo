---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSBYPRZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCUEeCvJPxhY4bNVtgZM03FMI8LyOR4h2gvxF6Kx102kgIhAJjutMYMK1mSlmbiTvZBb6U4eOC0Ecs%2F8xSosoikcTwRKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxU5yu5wwM7fUcEKhwq3ANseY0XsJim6pi5IK%2FNijVutH1s0UhoS7aMc%2BfLt58iAp9ErMEp4NqpTs5ecUYSsoghCCvxw2QvA8iUeRDkvXSSQ3RBcgxsshMatNI2OYbN4q4z8%2BTOBQaYY7yfyaaUxKnHrEL7tVKdLLIjH1d%2BdMHHLPSWfRr1HYoRkH7aJjI1au8Ak7nPAIKBzbf0t0vtbOwVsIdyVrkNqqisoiIvKXPm0QAe%2B1WCC1DxIsbmS2jT22p%2F5gOAkJQr4AFQZ8P2CVcNmw8UCyKzkvZwie2SJMwT8xbM0wmmtZraoUjqlyn0XILX4nOL0lehV8oFh10xBga2JS0qTS7FuljqszH1BdmV9SFbAVET42tDG5Z2rfVjWW8EZVlYOf9ESovJKdyiR8oKvdzUGMfBY%2F%2BJuXZdiuXETemXBloQsGmNszOkeuCxm7zwj3VHtulA97lgd%2B9fZpEASvw68cUsQW5T5DIh05HwKLI4Ln3n4hnYTtXqjzZZKLAd8CFy7GcchT1HalS4aNZ0BULG2dstgBv5T7XHG2sG45tj7Ai1UAIlHFb3SGTydscaziZ%2FdvFt3oVrU%2FCfS0g8F1tQGtCS%2FlsUtK3x5MgE6SBDoBQBRkbrJXuvIhQjf6hzkzFy3dNISgyj2DCXgYvIBjqkAaBB8AziR52yeYvXjKXd3yp8ECbcPMHkokZdqHztuLabPFHgn0E6n4HtckgRh0e1YihvnTmI8rCObovs3KWTxpGG%2Bg%2FYLZMbD5xcrjVyRYZi2Ww1gQXotu1jacykhwwtdvwVMImdOlIiEgo80JNj3R9ufQCEdsD73hR5YIIkY%2FXY3ojuyD8uAgNmPqDLEhW1VpAPX6duS9I1b8T3u%2Bfn6E6IPYlM&X-Amz-Signature=04994473323c280939ff8a587d6da37b32982fb08e014197454aabeb8509cb5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

