---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HFJLQGL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDXwRfNHMXEwvnG9%2BuaPheaqL7%2FIOwUcY15V8A3T0KWxAIhAJxTslicFmizZF1IK4ZtAVghhT2Ao3e9SG%2B9UNgyEdmdKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzy77JCxrLDSgBxcmgq3ANM8T%2FLwskN7%2BHQVGpSF9fQXlGDRB1bxJqjt3FXrJq7Bfjx6Ts9juE1Z9i0xwCU%2BaphPGf%2BIqXOkkJpod%2Bqh%2BiwwX6SxTpvo5hl0De5F8dTr%2BD%2FmG%2F2N4vtHWx%2FxQObydaeDGEOE2PdAfalJ%2B7%2F4enVr0wciq%2FMpYcQ8iRWVfCHmgKCrp32atfkRrKyPnn0eefdSZqXXi8mrqnhqlyW%2BOZnUNYi5f0eB7sQNcAWEHBT8UuXtnLovGI64sy2U8JEwV5%2FgefcAwurs%2FWQl3N6zZbTQrJHQCd0sTIYV8RhtmjRRXT9L0EhNuXwfZE4v9%2F%2FWQVCxjyobvUMPFvV9uOvExXnxD3xZE9c1d6MrvkKMwFPiLv%2F0CMmpC0wo7pMMtVoqGjcuFmriYWBPpEoGO%2F0eswhfXAGPhfbDqYhoAeKUmFtAa7dAQOPnfR0Udw2xNuU%2Fz2AmfwUn6uoy681H6nCc41XCOrr%2Bj1NwMn8HoZ3hkyc3fU0l4sK50Coj4dxpbe8MHO391xC2gP5FJVcpKORkdoG2a2GBudANfeKJW%2FSkVSzzEGFJmMssGn1mcdIQfgOLVyNJ4gpsQgotRLLzCFXAxB%2FNl0jhQHnBFsk7PRyFemr7qJkQmOUbtOzTk9zlTDWj47HBjqkASwhdIh4rw7IkQZ%2FFHPiZ1Z3MwZfuN7Zbnse8Icurx9vcMuZnQ5JOe4CJsfyDoe2mIVjiYjYfDLYmWwPcUtTokeNdrLxo%2Bq5c8yygBMrH3KooUxIkuESnIABALui7bJ3s44f1dCKyy8w2QP8ynbTTJBf1IJRswcnwnJrEVgZmDbz6A%2F4X2LagapOH7s0elrAXkuUfMfrUng27aWLAp8mGdSESXeK&X-Amz-Signature=b49045b826149f561639361ab556e50f7c15b6717df2ba4323baffcaf0e071ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

