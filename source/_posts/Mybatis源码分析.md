---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666EZB64GZ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjR0VIzv1QZ3Yf37Gxj%2F2uLpKe3bv%2BBbDIm%2FvMpDHSpgIgYTcezxOPiacycFiUCW5kVIv95WWLg9bKDkBTELGTmwcq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDMIqJhaw%2Bam3LlUPdyrcAyfyF%2BtYfDRzRQtpPPF3Zof0ppghHEDyP%2BzROqrGdBQhbyfgMxKhOFSGCahIny5JpU5qqIJ7QKlSLuBjWmR16cQOuz49nzDkUXxIww9b0QggeimcEEpVCtnaEzO11VBv6QyYm%2B4dkZ39Ep7%2BY2nBJYrKgwCy1IPkVyCFIRZJyuvvBHciNyVuyMMTJQu18WhTqef2ug6XrnI3uQADm1kkoVozq%2Fxx2a205%2Ff2ohpNoKyaXV47mp4fLwerLJodBw%2BuVIUqqPeBnuhUzRyxjZxnOJUEUUMjj%2FtC8xnwsf%2FOpsYrZRbJ1H1%2FAyzjeP6QSSn9hsAOapLwRgArWYwoJ7Pk33V19qX0FfiaWtILjZYQ6xbqyOpnAaZNC%2BLXgXHGfl%2B9xWQYiWZqJQn8VP9cyZ7y%2F%2BeVtSJWXhQJA4EHxuSvloHBUWKk0snGDrMjZzzgF95QUzlpjfv0%2FSPUa9aspJbWfdN0Vmn8KlXRq7xjUYZ1M0OCEizz2cY71TynXMSvaYzMCv9fum%2B%2BhwGraV5mwlPRjFJHojAreQEicc80sN4RPmT87otB%2Fku5ac0bRt8LbzORiA5qljYFKo4d2X0iEus%2BiPr7U%2FvAquayECoQwnC9ZFh2n7dL7wAa%2Ffpv1YDeMMiiv8YGOqUBu1UouOHNYDXFy2HJA2NzTvvPJglhgqx6OAqR6q17IjMf5wmd8Y1GouE1eN0fcL48GBJkp1NDwNkbX21jmVbEKak4f3JiP2lwYVyowJcozHcxkQdZdPSrL5HRCFOBrwplWebFMRjVf7mXO4lgjIQllAaw7fRvf3g0kiqQE6QYoIdp%2Bjav6Obp46gZMtWhqaRcgRLLuWYihIc2pGJXN2t25UOI2dSo&X-Amz-Signature=7aff63a9a2178e242538582bb8b89ee8bebf31a0057c84f54d0c06054b17c11a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

