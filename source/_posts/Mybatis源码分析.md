---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GZATYLI%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T040054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDtos9SYJP4ZxUGU%2FcDzh6lVIyHvUepfzbHdrXXiaQAiAiAlPyE%2Bs5C7cUpGcrrSsVkhzTS2mqHIra1KZwfKfY1mgSr%2FAwh9EAAaDDYzNzQyMzE4MzgwNSIM1IDm6n0DoxsfMLawKtwDyHm%2Bb0zbo8K8SWyoYTpIS24E1M65FypwvlIeJOFEyk%2FZtspTFIT0R3sQ4KXPc1WQxoODHreiXD8ciCrBhbFZN4qojT2MQTYL8bIf5BSDt9NLymbt8B2qLoXr4z7AKs9eS4sYXuqfhaj6daYOd8i%2Fh50dzM7UdhqWwaJAZyuDX0Q3yUJ5pKJ5plTpFufvS1ilVkOjTmcqhhTrrDVUQMKcqUiTZYyCqjL26VCALvuHddg4s8HyXigLAqkQRezlRkRds%2BpEjza3nmONwW79Iz0NsoaqBff3NH2mGo%2FufGUCE%2BLHgUQf06i1QN8sdxa1%2BCmmDz2IeZzkyEInPZr9pnWIaRjjprAwXq8%2BKsP0Mrgg4XkRYmypirlikOxY45G%2FIRBhGdTQJ4aNZ3%2FMQPDYtKEiAr1YQvrSpK2oVI5SN1BwzEaHIupdd4T7MnuJhOWyShVOKxt3xbvCHh2VdZ3OTEoY6g6XEs5YakTr1P0Ir5KRrOkMXKb%2FIHjwn7iDtQGWejcuVbMnKQuLFnpib8px2OwK%2F7UUMImEvvd1B5%2Fh0H1ejNPUfDngQdRkiXiVliqRZ9KzPlxBB3aPWWU9sDll6fcTsZ4YZWMtxIuTSoduR62YwchTaGwEKtkrIUJeSVIw1uqZyQY6pgEn8nsu8RCb6hh8DnGdJby71DmSXnF5Wz9QJMYOWdhvmoT%2Fmm0bg4ftkA%2BRaQyBigUrnSsJye84MJyk1xydv2M0p2%2B1%2FZMq4YT%2BxbVPSO0R9hXhCwBsgiykZ4o9Us%2BSBj%2FUwZCayuj8%2F1bnHcO8BuMhVcTalgKI1iaxs7YYArRASfLyNw9IkrUsvkVWWif42FOVHMgXY%2FbpnCXje1OIdSlmSFUwG1Ub&X-Amz-Signature=a3ade1b65fc970f2223463ddd4795868c2f007defd09915b654737dedd4afa52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

