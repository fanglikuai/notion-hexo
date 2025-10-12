---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQO7SB2X%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCk7ZYKtsuZALpxrSynEQx4MX2goaKip7u7BQb1jYUusgIhAJlh9JnLvjY8R%2FdEbqUYbdRRbJSKRELuh69YPEQQKYr7Kv8DCDUQABoMNjM3NDIzMTgzODA1IgyyxMaQ2rQbdtxz0z4q3ANDcbzaZxH%2BDL9Z8iAaE4X5uOfpO4jAZhWXlLWN9i5S1YeFTtehYLhwZufy9KoeBWqeLEz6wu6HFncBummYdjAj6iErZ%2B5F%2Bmbhhy%2BLGZDVMq1xh5y1DEx%2FQ0LfWZ1OMhCFao3z2iZu2rDJXx1kyLPHzHLv8dj4bN8Pqi4DM%2BtgHD2EaUTk4cOYeyv1%2Fvri%2BqtCiytOQkMYhA2lfF6NWYZ4Znkkf9PjH%2B%2FVVFRhOSZdcwXDk7VIFZwDYvJX0xsdIuQcdoJFKSDNEEcS7tFFHTc32ycfJGdvhDYCXXghtxAHYjKoFjOfyRLQcvESLHLEWQBUdAtOzOEj78RcfaoXRqEF0HR39G%2FqGgwD1wsVJdnP%2BtCpQ8YEizHeRzZ5Wk%2FpdQgk9%2FsXVC%2B6ZgSqkLxBwkIQ6VEgmJjAI8L93h3tAiNVB%2F0uEKFVn3ltd4pE89n5KsWLnb4i1pVoKpxE5Kfhvywfrlq42CKHNnBJAN1rfBQtP4YkhGJqNGa%2B6RZBxS%2FEnM67nGTExOhu%2FvcXBvpthA7eJgIyIywBlsFqJ40voWHTevugu%2FSPJzSl2logCL7a%2FfagL9kpjQeK3AiN5u3yenXGCycv7maJmEO57Wr3g5paI5K81ODjWNBmGn107TC0ibDHBjqkAQFuUpZMDTLJrDa12s5mfkjKSMHnPvq5ZDIjfPxLo2mzH%2FPM3srXU6NdQF9XHJ87wohUTIzmEUyonnNs7Mk6HNVA65YZaGbYAwcKmbOCWCequxlootFcmmFw1PD%2FsK3y84SxNDMSurjCA3UetWE72bYiZn2%2Fi4nNtRjK7XIj%2Bt%2F8HT9vjRlsk2FX%2Bubww3rPXQJv8r0DJtpCXFiNloPAppbghdK1&X-Amz-Signature=1fac2c664dd0b27ae59d552186efb672a0bbd2dfb99668488750b12d8a38e439&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

