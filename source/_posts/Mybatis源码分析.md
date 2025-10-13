---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653E7I6CA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1PsLzrufQ9Z%2FDTHyJopIFOCm3GfD5SzLa9zeUJ8P3TgIhAOM9zYZxu7ijjMacalftN7lhy3bIZkp1FVMpln9tJa1SKv8DCEIQABoMNjM3NDIzMTgzODA1IgwFuHThthz4cSF7C9cq3AOTORvbePFL%2FnCuS0IalEyzXgLTsriO1UNYlq3N4eNu6r2EQex36ehP4yWbUZ%2BM1BH7TOlSWQU%2Fj9Gg2cwxTMqJp6Et8sdjhprICiO605kpsbAefUHMwWSVSwfPSlNIRR5sJnckKx5AblxvZy1ZTsoo%2Bb2fXGYrJav6YjPeHrDj8gTwW2nCs09zzJHUsaWsDT0uRd6%2FkZZZ3SnnSVrGIHzfAQk%2B8QxdH2%2F%2BnJUEqzDpjYpCvYabpgm%2FSemNsUVFXBVWzKEZIXvdllBhlEnYmT%2BlQSIUZEGFRgLuKVdaq60lzflON6ve2H8T7Q7jrRJNRueheKwNzMMml7OsAxtyN7AcL2aD9oKOmVkQ64Hllvob%2BQOQ79eflwLwolo5Eqf5fcGa2nO7Tz25a2E8dANQCHyw%2BnUQxK8k1GoDleklNkj%2BLIRy%2FuT%2B988AY%2BF9OKbMqF7B9zeX70noIKdc8MsneeLONzvC4Lk60eWw6zzxQV6SX5HY4%2BLqzxUHbgOZRerNVJSaf6CaHxUOA4qA%2FI3HbEQB3piw3TLlxOJtXY7asO6k7aZ%2FAdd6wME0SBPaI0FGXtqw3H%2Fhl7lc%2BnksOWRCN91eMJKq4zRxeze3QAP2uCEktTlVBT3dj0wM2sAdIzDGgrPHBjqkAR7Eg0hTVwsUC3tOE2mx7mTEobaQAjgtNf2NVXBEE7QExA3zQxOKyOEuJ3CZU%2Bmc9JSqYDWng5AVfrUbo%2BDgFipZwhpiyIqaXWGGmHB3SACuuM1Dg0h6p%2BOgaggiTm1TnTV95Dt2tq9%2BSVs%2BSl5xUXzTyzYiWH43Qy%2B6FhPVsNpkVg3%2BerblNMs9TMVXQDXgtO8esh4d664I77Gc7v8BpvA8hGLs&X-Amz-Signature=2c26edf2855edd52a6f994c2933ef5c3810ea13ddf5ead8dab158108def4d993&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

