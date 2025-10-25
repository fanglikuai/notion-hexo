---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664P3MBZAF%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmCb36TJTK%2FLGcb%2FUQ3jQUo%2BAAt1WvQbdsUL2FHGWTGQIhAMMJNPzIuvFcKcH%2FdZwZ%2FSSiJUnkl7NWrLZWl0P2VDtfKv8DCGkQABoMNjM3NDIzMTgzODA1Igxo6Bb2xP4PRgTvsPcq3APlQZ2GGP7ZiRTFP66XH0tcloiw1nnUUQHCI%2FD1aIFrH%2FpKgme5hAOx%2BCG4t5D3DEEoFxnXSBkPoT0SxOG4qOSWX5IwkmBE%2Bc4kxzLSGM0CKDl5b%2BFQq5byyCCHLQjIS%2F1IxQxtTQUrtXgaE17%2BrQYVd%2BXAIsv%2FfzL0wao6qfV0e6FZSKjMzcsUp1UEHsf04zZav4MAFjIanO407mZnucaahzpKvRjZPsgBlJ7TwM9fftS%2FP9cPADpa53fv5YCttpBIeQVRDoZ%2BMqubHWzARISFIWgF5HtCH4Q%2B0Iy97Y3H5Pe28zbRAkc4Q7JN%2BGTSzf%2Fo9sqaY9As2vkghPjcvBC8SqeM3TNJ%2BxTGhJvsd1jPB6DCb3iVbH51bVx7At65ArYm4pmuLMRchDevtcS75G3bwDifxRm0cHf4rfn5fPtDfzPDr3mIn6fC5PBHP1wRCVKNQlMBlj3ry04AhpmUSuyB7uZYN2hbI%2FQagQc1Vk3jI%2FYTWNkF6q1JpWjfVTSP2LAR1KwzDL2GMWIvj1vAiAoSI%2FYtxyUfZbznswz90A7xRm8x4oI01rXtGyq5lYr7z0FXZuNkEQpRg%2FuID0H9yiM0IyVxloJCW%2BNwQZGMF0oJekWZt7GHyUGaL6ddYDCimPDHBjqkAfnOgb2WKVv7B27m8rnZjn4JU9tFqiijBIdGLnCm0dGx9llbMuydnPv7R4tM5LOxXTzjhG7q7i%2F3tHBTC78yquSkiYIPst%2BS43eOfgLhXnqbognG5nPqIrxm%2BscCZLzTxzsUmBSR8CNzLJvL1nq0eHcHh2BCk%2FDjZm7rEviaVsrQkGFoaL10qAF6nq%2Fqf%2FrBJ2ULQZrMD1%2BPnzdTZl%2BIhBkHtNqx&X-Amz-Signature=e996b4a5242d18bddfbc239bec2307a5b484ecf8b3e95781a28f3957a4d712bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

