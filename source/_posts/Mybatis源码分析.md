---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634PEFAKO%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T220540Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQCWKYU7%2FDT%2FFb4eqyPBzJm8D2MTEkJTizeFCCd%2FklSyMwIhAO0%2B7nlKDYXEpsNJvxG7Mm%2Bq%2FG4fApsRSpJiwfvWA5MSKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwCakJmqU6Nq3grs0kq3ANI8ffHWDBC7eLUzZi7ChcJCUccbkKsEIBMJBYet6Qjw2F6dXAOMJAoLHJ1owOUiuCJaYGXzXaVq708vWXqzNCd8qLqyrsll8Hpz08dqTUJ03g1RdPBOXSYO6YT1OWIYCgRnSgpJpNJptek7daW7Xx4LLAqN%2FXLHQOIgKrPy7IvZsjpn%2BMm15%2BSPVgWsMVORusqMxJxZiR0roQOez%2Fe1os7zIWxFIlzp95TchPQjnMessYl%2B%2FtO%2FkPRrMdFqt6IJyiH%2FshxjmLyMoJumZ2UspbbzR7j37hcEVf2wBzsvSngYG5gNgRJ3hP0eCwgwTT0S6JTTRRxGdZdH34UIcP5vjCW%2FBRdBBM72fkZmF9%2F4zKpN6zPIeGTuEmKiNp0TP5vEO4WqZEVP2i9SA7SOgQQc534dhFtjEL4sAquBBkfks76oihHuGZIVDL%2F6Ckc19juVyb37%2BypegNZ%2B135D5PSiAauaJuMrHRQWR%2Bd5d5awVp9UUe4R36%2BXFrgXm3aPJEY4FsnFZWlrrAXi51Nivh9NRsuMRBlkfuH01hQO%2BtnTgs3QUlpmetoTHaz5LAIfNpnsOxruv5ekWzBQojESSRrzLEXr92gm5VRPjwTmN5JpQSKdtrpLY3uBhQYgcvNkjCOgabHBjqkAWSaS06L8Z7xphPYwyw3UXzvbQ%2BgY%2ByInwhzJbFruCXII4HsCTQGh8h8G2tRtqKTg4wc9ONv25I3bvoeU1rCKmPnnv4AT54ATw0%2FbMdqCl2Z%2F%2BQJEwdnbSSxKKUgftUYcs4ALg870TvcvZXRvd2NdfWPeoPyVLpKE9HPeR%2FWoGQPtV%2FShhVEYL8pljnrYrrbPvUX%2F0B4SNtWfQsYBPf5n4QMhm72&X-Amz-Signature=e7eac0b4b0c97dfe1bd927724d48e2a8c3d93f7e98f5995dc6dfae8f66c0d98d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

