---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GTAJSND%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIELxnRB%2FFxjArzTwDaVx5UXh8gfKOez4x4GG3Yr9S1R7AiA8WZhyOvh8eJJSKxty7JcC%2FnB2R%2FtD%2FTPJH3B0iBH%2FaCr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMaQy2WwzCnvI96nFRKtwDhoNnyPrv%2BjbSsr0kN3kVR0Sd%2Buo9nwRtrLWtQlGt7f3nzQVqJonKUozrVCvfnj0BOTUtVGEb1FvQMkYt%2BvtBI4kN0IVmkUkVbI%2BqFEoQJ%2BvFZsmPe1pdzpcO4fXcpmmwE0g15byK1%2BfCMN%2FY%2BpaTYHOQ4e6YfsaYOxlRZAgUotAwqUfyKE5on7XMqGhIB65RCf7dI7GuVIcOQm%2BLYv%2BYXSk5rVMCtULJPaghKwjQRg3ZZljuygUB0Z8hnjI3BaYa%2FPcCE2GNTYMkIBCro6Esl5ZXoH4kzZ0LXl3Uv8HgJDvKVR7OP853LFXywPtXBtY3frs%2BobBxkwcWadIwJ49uCib4ug2csGfjr7iCO%2F5wj2wagnjD%2BAokun%2B77yETXzvILpJprOeMiRRGikNPzUirIlzvpuDatXKV8Ed%2FfNmfb0M56ntXzLcockWc1xmRDipOWdv5XwdRAORKj9z40SM6fYqY4PMmV6vwA0naZeMpRudOomUdqYrsYAhYcAWxf4J%2FNgYsxsO2VcDljzTLQ%2FYIg4rYy%2BzVACqOh3hciwVwnJ55cLB%2FMzR7Nf3V9FfiezCueI3Xd9rOxf504cVy7A999%2F95v3kKi16HIXDofYE35Hwh%2FDFs%2Fpdwa85Pg8Awh7iuxwY6pgFAnL%2BK%2FXg5%2BsJnGGq63VPJo7k2qT1vr19DfA2AUAMBlQKTRAEO2sdBkL%2BVgr%2B%2ByUyzLtXLzd6Q5VEgw%2F0kuTv86%2FJbO7u879ilY6Jfxkv5AbaLX619QM38LUFLQj%2B4UpcEBXndm7uibYPg2tQegRKJqaDeJizP7GK%2BZL32CmsFAwZHupy3O1EGBDVi61DEdKtCL3u73LV2SxwOwGrbqUjeHCx6UAVC&X-Amz-Signature=bd5ca119ffc5f57d6d6811705320b5914a5427a51307b909c265c12ee55f5385&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

