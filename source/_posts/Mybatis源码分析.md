---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3RN7SFJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID%2Fh2s6UhLiZqFhxsc1X8nlBqD54jBZ0oroch2w3ZE%2BrAiEAq0DZCF8Mw0RxogtAwCTuBglfp%2FRGdsM%2BlogGsXa1mjcq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDDTIfoUnflaao2F0zSrcA9WnP3JkkFAJpXv4xkSrsotMB4PJ5X%2BLHdRgp14HiwIKbEJWyBTMWZrNW3qUtH0dU%2FYiIUPcVdC6acCd%2FWSjJknwx41tLBHHrrDnG9hciR8kZd6ghVMHRYXPOMdl%2By1x1jIZXFce5ZMGNUQ0sqHslDmA59xyQX3RK1ux6HX1eRocgSceGLYBfrdGAGk8YF5RqQWSQyyCfXZVZl3XpBnz3T3hLa%2FOHpEGIkPZEPCUqhAEfZJymZZA4Mr%2FzIHvU9tZQUyBdrsOJAT%2Fx0tZ7E%2FNjWmPoI9FFoGL92hxynXVGzD1qc7Cy20p16%2FW5j%2FwS52f%2FGzFlD2O733aou9a4GJ1wiBId%2BYVa%2FiDkdhBoaDGm0%2BpLB%2B9KckaVXxx1VYgMWW77vUDs1G2vRZ0vtS7Ck4kYJBj4odSZFTowcRRVe8a9TStBh2qvThd2aP8Z7nfgcJSi6pWPWb9Pb%2BMgYgtldBgEJ0lk2l7qB8Mejjhn1EAnm5o2W2rTZR2JD2eNvR5vLOVh3tWSyfPyIihJmRQu%2FwtlhHdLADbVcuYJKu8iaNd4BKQuWlVnh6ZmJQRn03s6mS2R%2FmBbAIJhM06A%2F4JUyOxTfJ1JIL6fZSRNUANzr3E5UXWUE5L%2FM3D6SD5sieLMOCw2MgGOqUBQC6y66u%2BCehS%2FfjUR9RQ%2FHEefi7CdFLcZwTeXhXefs7tptoolpA4v7iDiY6lR%2BBXmGMpGnk%2B3mE3K6uVYMlxV44UOFaCYxRK2a2gKl3sXR8uQ8OeJczhs7RGVFhE1WZbs%2BgucHaXr7JNoE%2FpNjugBV6tG%2B9Wn9MkaOSyj0p%2FjygN%2FehnN9zTArlv8jaR7BnitrEiqNjQ4y3wtUAWcH2hLSR7jTxS&X-Amz-Signature=b92c1ca4ec639eb3a9e31124965e355f4a0a2babefa505f2b437e269db3f0674&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

