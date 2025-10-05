---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRZE4ZMW%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGK9lfW8odUXyIlVUzB9XVmMKIavzMqtMlWiZGr0uevTAiEAn1iB3rPKAnCHWpTxCfLB2sbNCocYSVfXPEhwYs9B%2FlYq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDDFu8GnUmsp9Y5bxgSrcA%2Fvh%2BXOul%2BtvnjO4vasfrSOJgtgRKL74RiPh81p5YUTbCXK1vtMEnz4bJyFOjSiRPd2YoKpvbZfHigIhiEm0nY%2Bfi%2BbDOONGK3G5Q2rKcgPDr9FdcRreZSjxaQbl685ufPfcv%2B7uiBmAb4muj9Ory4OZwH6Z03smLEJshdj3T49nM50OpbUv3QSDlpOAoN6c8sR7TWyRVLLVN7nq1kHAnRrjblQLUENDi6DmdlNjxCdOgV5l0w3ApKgAUVeReK%2BTjHythUOr6hJptxFK46br3xb1p5Jr83lM1BqSXOK4HvEVNAbm6cbR4DPjEkVUMTHk0HL259ptwO1pFNPRLluWyOnWikT%2BWL7HD8V8kH9j9%2FQfz1DeYpir0xhsazZv7taaAb0fTJpl9rN06j6XBEjbhyHD69kIdAl6VBkbWAhtFb68El2jEKf091VYzWf3c85Sq778bxOoK1dFlZlb9FM%2Br%2F8i6elHI0fFrY7c8dGDA05PCzR8z21cNDiUIhC0xqPQWFumLl4ZmDYVk4jK%2FKRQqVo8ld7GK2nloUKeiMGA8HhI8mrNyiBWvepIxkmQ2rlkUbOfVgf7awravQs2UIeyJNouw2MEE5dOpNxHT1Wo1vamKBO8Fv%2B9creyhojgMNLhhscGOqUBWNe6uWYeZ2oyWZALl21R5Lers7%2FMAPBgqoOh5Fo8Cu77weoRPOs%2BvL0QusdTiX4VWEbepvVFbUNOEAfx1TUSlxFLrHfHFwuwsMGCxgwMTlSfFzdsbMAp3HN%2BNN9xJn2yVkjEJUYx6ZuToWds5nFTeP4zDbxJYsId9o9J%2FBGBwCWYdM2BxohSbpVQVFl1HOfBkCL0lK3XWBAN8zcTe9g%2BK87tRpaH&X-Amz-Signature=722b874363f771894b3f481d5bfbd9c3f3eb634142d5a496908e75d116385f3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

