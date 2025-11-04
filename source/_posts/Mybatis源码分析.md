---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUEJP525%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIACWTek9%2BjOuX2CgoZtXSLqSg%2Fl5DtJNzCM4z6c3D9w0AiA6j3KgUWYCCQlssDCtdTNG8yuOGTH0yRGPzrfKO8ZzPir%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMndUqGPujPjgATe4vKtwDvLClWVzgw7tU0sD3Cs9NQE9zUEjPEH7h2riWXmP%2B4%2FOzpcG4yMTEHcNA%2FuSBKFcaFeZTl3I37Ljwo%2B3LHq33tz2zSvv14%2F1jrweQUxrk5NJpPc2ra4FC2ASecDM2xbeQ%2FyAJN46PzFSwSkJyFkOfvKnAwo7QuSn2aXsuW8aCR%2BWFjnRBmikhwp7cl8%2FpTtC0f4UcmBlrU2IJGuDLuyias2GBqrMZxG5riBEEz4UAD6EmI2%2FGk9It5DaW4jqokenS%2F8OZOZsZzBjBvau%2FOA2%2BxArY1QCRA2kTvfvcVTg9orpbspgKjTnHZuQef3Z9DiiIP6Y92eZb7%2FsXMGPvr6fsvY8jVLH8Vl6W4k5cWX%2BwG0tHsGPy%2Fwsz8haRady1TTdqjtOWvM9trWGwVD5vJi83RRxgfoj%2BA5ThWWz%2BHa5YOJ5UHsyX71onrJhC7ATbDHwjADinsDHgMVF4DD62XJy%2BNDFPb8t37OasWR1gqlqy0C20PVUIg1P0w1MVQfcltrP9zldiejia5DGeFhNEyBfuQakfv0MFSeO0aXjZFZ4APFFVOnSj3ou5wzuIFcQVD%2F8hqfYzRm2yJyVofW2k%2FGdaelEISWnEt7ObdvxvRhCuRePq0JF2sjQRjIDUESQwwLmoyAY6pgFJz%2BTp1CkZVO8JTTyXg6TwVOQ5ZG4AfCWk3YGNLVTQf8907c%2BnSbVGIcar9Ab5KBIjv%2F7xidALWXfN3CUkzGkOCUv%2BoWZQPvDtezO4sA1kIhqHYgYzy7wxmu1EoU2wbVF%2Bom75YyXYp3Vp6w4vdIRSl1AwrkM08v1o0eSu5wa0wTdpTcqMGJy9lQzAwtqIKXD3zTxED2iZyGkMJ%2B%2FCdwbSSHkCvrda&X-Amz-Signature=00d45093ac0fa7b7381f30ff7ac86f6774482e17cba2d603afe94f91e330bc72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

