---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQXKU3J2%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQDva5bAS7mEfg90nHdR47SJIhYzkoSl%2F1Q0eTwg%2FyMFMQIhAIDlrEV602NWNk9NT31IkXpLcP2UxAVHV70h1X%2BYQyiaKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw3I3QPehISJsns%2Bh0q3AMVzb0n0NPO6ldTe4s71Hlwdz3fF8CuU2cddbGfnMoVA6M%2BrXqN0Rl%2BzTnBWmsmGgPeaSChFmvQCD7zwTWsjgTUTFJLAGLSJKWKLSyOvFN1y%2F%2FqtWScHVK7dxBDaCUK0HhgLWClQUvJ1Qqfs0sWmKfTznl0vcLJfYV%2BEWQi871EIWuaZdgzxUu%2BYtYuRES07MPz7LefkFX1lSkDfOhQ3yBwr1P8BY4dksLXRnv409M5ke1H6869Nn2gWAlMEBRCFXt4A0hwqKfbc%2F4ECi4iRvVpfXUjLMMRRqt31AwVlhGAX1G9WUmXzZIZzpn3ySMoXR2BZBMYifcUC0Dmx7NCLUoanmDmVJMKbJxkjS0vZmg0aprylQmwBOwA%2Fj6w%2Bv4bnjAoPN%2BarpJ6z9G%2B4YJ%2BW6gj%2BoqukY1OkFpwgqGeByRShksl5JYimXl3oHRMSlJWDhlVLCLN4Pcj0WjurAcxuHJw5ubxeVCvp18eWH51Pm0vDN%2F2Gb2TYQ0L5MCGvaweWJUI9MW%2F1YDSsP9npuAIIYI2z9i9YST023ZvCljcpMGk5KvZo1va%2FnM0fq9cl7Qx0eYI1FvLPM87cZiHT5Il11N6qZ%2FiB3GLwilK%2FSf7XXLfrLsfaVOvPxxEtlwjzjCyod7GBjqkAYgnwMGds3FKAIWQGKhjqOZfHEtO6UDTOktP%2B1OouVJe%2FER3uVzPb2j5b40fIrHxbU%2FBoN2NLqwVmCQX0m8HJRvJIfd1Qm405ooT45P%2BThdzu9ME3Xv0HR8hjpp2qUAM%2B6dARtSggPzo%2BkaH6bhKsH%2BPBvVVqSvQMKZJTs7qTvygQml5qY5jzCL6vISgDNBP6cWbc99%2B%2BzXl8NvD7mkwVVm5C6NA&X-Amz-Signature=7da264107499068a50f5fe871097040c97422808279ec1739fc1105b48c6742d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

