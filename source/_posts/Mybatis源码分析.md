---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHFLQDKF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJGMEQCIDWDAGgJNz2xKi1SAlMqHpjdZPAq1%2BkF6OnnXP9EuEHhAiAsNO2NxsqB0B369crqr8u%2BAmqTxlQV4ZiobXN8gW91mSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM82RPoEl9ep%2Fjvg5xKtwDBPDf53OhoDEh14vih8ufvUcue%2Bte46eReWBgh3%2FNylcXgGmSzjPiPYw1hVSZ3UMbopWwjyDedZdSABGWiKuvC0FnjR0mLs5WxxyB5kV82kCGSXvorsyOhC%2FIPoYMLL7cq%2BeKAxKjZZzKa%2B3NT%2FxBk82LhS1hCh%2FleI4GfmFv1UhCwAVgCMc2%2FOXdXRmzmRPYC06pcyVpWPhMgeFLdzmmWXKXZ7HM0%2B%2B4qbh%2FogzxVf%2FiDRcdyCAzMQYtu9XCc2OF7PBO4FsYPBU7hQjJ4bNs81A8i2ran3pMunX6L296XH5RmTY9An4Fve4mpJEN8OjSnDaPLp1jO7dE%2BlvUT9tZlOZvpGyHdEMpT4%2BnOYQdyfguuJ%2FgLpGYMZ8HCG5X4EueN11AMiqCedyIl2yERiQ9S5UlssxaIvtmpLOJURzK8ZLVDoiecWH%2FQmMQtlN1N%2BWxgvcZcCPV%2B9cbFxJQV%2BDjy6Pi1d1pzNnX1XHh2Y4Pp5fqYmCWtrY9vWXeEhgE3OldWJFqit7FtLhDWR6KG6c0X6OftLUcZhB9Tu8PxlU8VY4IiJpG7eOZnbUqQTdgNkB9LekM1%2FQa7AG0RdJOSo9Mn6de009KIGOPOX%2BNfsYi5CZluzQxd15urYR%2F3FQw%2Bp3axgY6pgEGjB6wcroe0uTXZeUg290rzeRZpiMrfyVlDZ6VN6TjSu2tlIVmrTwYJFVzEO2R1y3Fxc8OYB2%2F%2F5GKPQ0pqdfMDZog4NLljTk4n%2FAUbxm6u70FGcYNhMIRjsUoePksbto4vyOuxbzOSYSn7phOTRKr%2Ft5Meu%2Fu1b2NCqV%2F7Q7bcPl3m%2Bh7uLQH4rb1xI%2FGqYtLw%2B%2FZggEcXiSBMMq9lztHn8VnHGlm&X-Amz-Signature=057bd55b2928ae3acf18fedbef03b156c5edf54b3f67924737145edb781d8485&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

