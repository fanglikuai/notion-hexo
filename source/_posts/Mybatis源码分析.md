---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674JZJJHG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQDz1jFbk2HPxZ3TnmrTdsm08Z4DeNVinIYuyVIuVTXWcwIhAJNzfaaxZoEhsHw3ddA4pZzONnspjDAZedhwQWaO8yniKogECO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxxCLPPzYWiR%2BHnx%2BEq3ANSzxtXNR9deGFPvMIuUy8JIPiQXT3y7NHal%2FpBtewDLYIykRA1F8lQJptUNCf7GprygeWv7R2o185s2vNhwzLuLP6igjEFouLvLub%2Bjdu3jYlIvf9n3Uwa1Qdui74dm4YUmJf%2BoHK7UiEXP8Tpw00YcitA%2F5Xt4v6pBTspgvfUIXoM5miV%2F72EM%2B23p3lZjqfK3NhNqaMlAtnQRb%2FLaeABxLFB6EXxq3LF6EYfjGEbkQKSrumx84erhjnQGkO1vIdryCqCrWYTdzqRvJahZdDNJ8E7MjUlM1E0wni39lv1axWaF9KxkyEjsVAroq2B5RdFTGilg0elgp4ZsewTU6uNgwqYx78wluMWJIb2Cgu%2FdYSitfohourE8bUyXZEtvfAyVA7uRhJW%2BVi8gt%2BgEcoLJ7paVt%2BFtZUy%2FyYt5RzWHwfn%2BVrWMAoMkOY2NBK%2FpZNrnEli%2FNEHQ%2Bgi9Esm5l%2FQSsm6QAaGRtwOnOpB%2FzcfmMcXOuN18AItCUbwYpTtzGV5xLcDkCy9GMV3X0ql6JO8IoWdyVM6z26rhYLRqBOvEKax3tFGPDBuji1QYf4zQUKGi6HcMyWJYA3CA7zru%2BslNsibzvF9L%2FJNSp1Hii0z0u38ie5z3akk9b2RnjDOy7rGBjqkAZ9FFm05hDmcqrTLbcHkAoJ6OtuOm5mb9y6JN2FKSnHDSzSXQbJTPK%2BgLRnGtUYndF%2BYGXI%2FfPsVOVJs1%2F%2BupkpvR4azRHSJyF9MSFamDjnMlXfnNfhsw66ZqBU%2FKSZxYuUjTHPlI8quPbxTI3tsS5SecSAj28lN828zuHkDatLW3rdb5R%2F%2BTM9QS2HbB6rKPkaW15kKUYgYDZMWIjRFN2u%2Ft2D2&X-Amz-Signature=f2e4a406ea694226dd9a88a6c6cf86750ddf34943bc49eeb98313336e32aa1e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

