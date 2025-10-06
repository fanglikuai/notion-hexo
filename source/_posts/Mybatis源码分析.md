---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HK4RVW7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDD%2BPtDuwHITYb%2B4%2BjaWFEnAgEcLssSm%2FfHm84H7jzvGgIhAO6pGVUqiPXK0Z6KfcHJzQpZZrTFGLgHUHUPTEsH2jPLKogECIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgYeOUY8unepIgD%2FYq3AP5Z%2B0ClROo909v%2FkOGvMVJPb7wZbKqPM5x0ek3fjJyleVQjPkjNI%2FSiDSouFutXYqGS0hpQvPBeiwyiVYKYo0RkGX3gh9GpWr%2FO74Dws2kd6NmDFenVp9Mt%2FGif1hdHeNqEHmGPLsUAHSO5oPZFEF87YjBFC09o%2BdzUJmuDjYOWEFC6zPWAnaYWkPA8Se1inleBtLMTM7%2F8MExA86DgfSIVVMJdWdLAfP%2F889%2FQrcW740i3POCXSuk0SskH3fC%2BykSbQkFK3p53Mk5y7g9jIcl6EcZfgv9WsnCsP7CGvVL4bqZvwc9FcGnecYJX1EEw8Rm7Oov5qnmwzopjSbThEiBqkwbUBdBqOII8WgiDR15wdxKWAUcitnGtsO6VVfgbLZLeWeQWGMEkC5%2FPL9fMEz4mUie4ABMgEnlE8hemsJfa125obMxUBE0h6gMnW7PDnxIzUALRfFlWX44%2F79RwjnA6whMsrVWjVYuWYZeIP3iB0vCiCEw8Tv%2FKBiwnF9tadZYwBYxFU5%2FC5Zcj5v%2BKOQuBSKGeMXj6DEGk1vRI%2BDk7MQpdT7xJoJkOEUvB3463urGt%2FpNRm6MbzhAikXWOvJSdykewghyb0%2FatGMLHoy1yNCdvlql%2BVXrs8X0cDCQ743HBjqkAR0URR7pwfl9OCMhgES%2Bj%2FqDDw9W%2BhTpjJ1ZmgAAJ3xf1uFNALXG02J4SaFmIyklOFdOSfqgKAcA7sgqYb8WWAUO5MrZ4wFH3y%2FKVw0gaDPuOrDR1c6acsPYwB%2FykvgWSJGOFRQ9pIaRy%2FzRA6XaItGQ%2Fn4%2B9k82VzTWOy65NXHJ68Nzb2Caa7uvIDrHJAR9mtxJatxcmN0NpLNDExInjIsmvubW&X-Amz-Signature=779d195e77fcd78b0820d944e560f271eacddef8fb3fb6a1c72c9559dbefcf89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

