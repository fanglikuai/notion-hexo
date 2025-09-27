---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652KTZECL%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDoyzbYPDW8MtIHoOb%2FYWp84rJ5INwEULFaMuxu6wy0wQIgMn81sSH88qAcuZcFIpL4xP9kZB08z07eODycC6AfGNIqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQM0sN%2BmlD6lf2NHircA6enOwQ2DxhHmdCCo0gTIVRfNHK1OeOxNscqW8vdpGx9x1BRtHjNINBR6hqI8pj4b6effG%2BTMZK0KIaVtzgG0Xy71Qc%2F0AShQ9iWTr2gpS6EmhsG6ZiYFfMX3mbN41ML7hjieLZR8WwzC%2FC3WpKFeJ4xICe%2FDpdBUSnt498eCJsO4Z1SNODzwIel78l2t4XzlvUAQMkXGeS8BJwjNBGil45c%2B7ZShy9BTXMkzlHVvovgNDeHHzTMKiSDqkxgYpOailDOH1LQ%2FMs79lJAWTRVLtzeSm%2F%2FhWIuhVj2CX6rQys5tcwVJS0i7sWAJXgIJXeoF%2Bq%2FRfeQVSbqJiAUDilrT8ovdtH%2F2sHY5KltSHi5XmNqK7ILAGNau5Psdf5S6NGxh38d2wSDIyKm4BlajqBpBTJa5Xh2HKqN5tL7ONe689j0Ao%2BYaBRR6OnWGMshw4bmGoCRpodSncKJGgUpqDTNe%2Bx%2FbtqzLH0jf3hAdl83HK2pVqTPjun%2BoxxK1unDcNhuOv8jHgjJp9COw2CMkZzFrhM2av4uhoRA%2B5mSMgNdJklxvKzO2tkIQp9tHqL2nuzPFcUzbz5sLgtnXXVzhsrBdFhLwkq7oXFWH%2BtoDqFydRjC%2FYtdxyDvv87QnRp3MJez3MYGOqUBGKDcehQYHTSqUzgp%2BCErlhkfpmi2K901Sl85oPQolQuJCN14xZ5CK5BZklTUmrEbV1pqS%2FXNvUta%2FetjMcAPGu%2BoKetPUB6A0qkoYUutg7AbAezxW9nZMvprJ%2FvWycV%2BZHGwckcc8d%2BX3m5JYxh3gpsdA54taScQIysUrt%2FIopTr%2FTGlzuWwqqzOntrkEBgK8GmNMeU7yFMBkMpcn%2BkTjiNPHlrK&X-Amz-Signature=581e4499e7d933a2887d7c92634519bdf927bf028020a4e3d2e0a284d6c793bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

