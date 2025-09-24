---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3H5H23D%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNbFbekt%2FW5CVxbVEuAvpFu1fsApsVA%2Ft9umrkQ6LdpAIgN2JYD2HGSa0brMzK2VKefNFfV41s4nFIsSNnOa2yNl4q%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDE4remX5Mus9%2FZ09fSrcAyk%2BxIP50v7PtC3%2FyKjYBXE162ix%2FyDYAT7hWSuBBltYJCmHhNYdHwRyErUeLj9h2oSuZgvYaYbO6bc0LDpW0Dv07WrES%2BiJfWB0rUHthIo%2BRjZnqjYrtBDT5MBfnWb9GQ6iuWAgje1gMycUZC%2F36e7ebCxGJpnYUg%2Fm7fNQm3qPajcPVxRa%2Bj%2Bn4z9GEcTjBz2wpGB1Z0RxJiROtHmyEIcsg64R09Hvjf2RPuYZO4xy3xpA%2FlyQcMVKMyQXgoySmrellzRC88TSuD55HbXq21CT4kHHcIUeAJxF0TYpJdcQ6rnfQpaBj1uHI73wJN%2FsUQXR8LcM7y6b4H3c7PlsSzMGVnsY1vz1PESJHHY%2BkMhjmFNIIzg%2FKLE3h5OO%2B%2BXmVqZYynELB%2Bd8ZET6ZGRuxep8eMJIPtMZGdnISrUPiB9RqfogzADo%2FEJCnbxyzX7C9Kp7PoQwa2aCnyrwy0tPasKgiTlQ5RReYGRACC1Gu8hPDeR%2FfwE61ii1oW4T0v%2F3sIiNmH7x8n3MsdOoDih0P%2FTI0plgGyRx3yF0G6xj3ItxZIVt2d96DNcikLruNbFU5dc%2B%2BjVwK2OvyrAcS80CnIOyELGr%2F8hX6jcjqRpXAu2HKqMzFXjAj4UIIDVcMJnSz8YGOqUBLXxiHTJUQ2d1A4KUDFYLDVDw2qiL4LnIssSzNWLO3IMSo9K6XqS%2BhXlnv8I86IaigE8At0oHkeBGp2aOSwPPIB8uxMw%2FvlfIrZ3FAa0%2BUE3A4BSo5FRDVliCXuadfIdJFKT%2Bn43%2BIdzhsHLpW4bZiuHMXrQ%2FA846gw4X9grm1IPN2U1raM3sbCGxMWyacQRfiasm0JwAhIheRLiRbtOEyTjyXH51&X-Amz-Signature=c6fabf9c7c769d46f68f28851f41c074f2660ac43af558de9c5bb591ac55ecc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

