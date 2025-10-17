---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCRYVDC%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvVdKdPslJOdBTWPL6InqlcMQcuXQsD6t3f8s589ePXAiBFbbjO73FV8o12hzIAp55nOWyXnuWfciZgFOWUV%2B795yqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMVkCHh6Mhs7PlLW6AKtwDh7h74ATpYXHq%2Bbs2%2BhqWvJAccz%2FhDCkMtxWnPEXfSzmCaCrtuiSf%2FDRgCED86Uq2XJba6NgUFF1XKHPT5GZpBo2l7XFxLXrWnskTCCPt72gF2DQyyCyhJ9XlDhZ3tCNB3JF0fjftQarVIMFK1qHlmblfzsLrI1B5g%2F6kgjzp6qR7SOk307rEguYg16TjxTNACjfZpIp5z5UkXRB3TIQuU0PU8UQBFbbk%2Fph%2FSHcPEhHBZiRmdzbrmaAMKxee1600feqat5xTOb7LK2jAj2aVqcX1AEyjCddwcMYl9RbBebvd0JczaizIPVQVVAV%2FGU0HmIYbU5q6%2BLZ6ybxYw1J3inscYp2du3avsebwIywwbLQN3mxVkxMpRlWUEN1vC4vgVuqmoLwEhj5NeHnXfKBrLCZ0h%2BMg6851FZjTvXTTOHtd%2F4THfEoJrRSyqttqF9RFlnx5TBzIRpGIaiEVFGX2kqDQtBS%2B9kyjeIkO7qPizLIPN%2BpbAsyGN5tvT9bm1ffZkg81Ct8CyOzdit37iw0tnmMJr70E7BpgWH2EeN99Q7IaqquVBGtlPXNWeydvxBMLOBrU3MMBkFjsoyKX5ZDr5JES7FRlurMMv1AOO790nPl52%2FRmWYoS8YWkm4Iwo%2FvFxwY6pgG9ZRYI23q2ks1Edv31kU5FFyWJXoBQASAngp4sW61vN0rhlvaYfvPoUBhg7ydFV0jPi0VyBiEJUopaJtQCVg2NecvEMKLCLp81x%2BBK3rxraFmxh3fvEZFHSgASNnQxI3jO1xsiB0PFNgvAYPWA3Pf3MAbk%2FgSrLhtbo%2FL4UglWHJs9LWaQCIo6GIQJ%2FTJ%2BlMrrGD9ItwMvl6UxYpk%2FBgrM4hnq8%2BEG&X-Amz-Signature=3cfe96ca147021cc3728f3de863ed37849308b3e396a24e3f1b54fbc29885579&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

