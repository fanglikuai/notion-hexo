---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBGN3DOI%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDLxSzgAdeSCJStTMx4BQXcWQIpII%2BxJJ5DdFqYxUy%2BsAIhAOSKMNA0JMFm4EMZzZcp%2BzErwwp2kquwD5Uh%2BHDGS1aUKv8DCAIQABoMNjM3NDIzMTgzODA1Igx2qX30h96Wd%2BaAsHUq3APGUq6aiEJsiTcFI7%2FEYhYi0TV5CPblZpF1bWbL9jEgUEavh9zHlaPbe%2B3gsV4v1T7%2BFZzOr13bNyk65v4FXfNLj%2FDARxWh9Dhey%2BSuvA3OwuC3ymbRUWsLqRX9p2ba0D76MaY2wMUB0BSLxhGCFscx8jlWdLM%2FI3Mb7U4ZmfJXebMQp3Jxlu%2FhoEELzSkjOPpN%2Fuj28z846rqev%2B%2BhA82DNowXggr2SXL6qqNYwTk5Tge7dCoV%2BqCUuaP7m20DG%2BDnFDYn%2B6PAzjpCHbx%2ByLSY4Kwc9Fuu0G%2BkAxBig9WKRNnbA%2FZfUPX1uzAj6zrBOpzHmajlkH35n%2BQtbvtZvjoOqH1qmaX2zQa%2B1ZZYLv4edag5AN1o%2BIyZ%2FCGrt9FVBqk2yG7zKacTn8TFvrkmZ48VHhKmJS1T1ex6vGo5CCDy1XYjE1yadgbMzf7fu9zHGIzLORp4dGbC6GpTdi6FfeQpV2sw8aIOmVaRHHGdCpdb%2BIL1Rv4GWgC9M1r61f8Kg9pCqGHfaWNP%2FVxBcnx1Rq5YjgzJw%2Bgwxrjnl88bZe1lkyE49brIwYM%2F8FWvae6y92MdsvLa77XY%2B3gVT3B0xzG9CXtJzhG6SjDEgaZtKK3TR%2BNzcX4M5uoNHeX6XjDY1sbIBjqkAfbuIASCcxsRHGuCztfVEKnE4Xb9HX6iimsHEXPmJ%2BZjYX043ALX2ekkUlyMX%2FkVm7UHelUgVWTLNR6Yibx1iZNDKROQ8oDCpocMfmqjo2su2pLTjnDcVTACnp4BY7KzeUJ8w5kGNmXiCYDoWC6lScQLSMb2vsH9STMAlEM3JOVItKETzQEwmmJva7uNUzqXMR02jXrhh6nc6EoYc4m5HTO2GUop&X-Amz-Signature=90766ba20a2ec9b6445181afdd1d21e4859d68fa5d1289298545b1ec6cbab630&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

