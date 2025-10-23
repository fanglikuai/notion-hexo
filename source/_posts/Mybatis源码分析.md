---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMZE6VG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDPQ6AEtmof3z%2BCQWS7DlCr05fS3fqB%2B0AzB0pGPzlkLgIhALNOTiPo1ayvngwVilWRRwnd0Vh3JLVBUENa6TQD9vfqKv8DCEQQABoMNjM3NDIzMTgzODA1Igx23GSK0JK4VWs48sMq3AMI7rDDdUe7LXZnuRNVb9HL9e49GXLwgbhToXZqaSqGIWHyjJt45DD25lfXuxyCy7EjT25KxWkkuh0BiyAev3QCh%2B0bq%2FMhqJQwQZiux52mX6k9Ytl99vOzfOwolKtm2DI4oInx9992MR8orYTBVsRaKxmwBcgqtpIBSDi0KncWfNfe9As38qXm3n8ATUWV7nMIBTeCccYi6uSODNfotHLzxLhzKbhoE5l2XzEPtU8tQXppY4fRntCyl9jJw76c3Cp3O%2FQVQ5HfjCb8YsPy1yS72VwpyAixO41Z3Q1NxtH%2BraeW5nBlLFbmS7vJc28tUI8F72x2K63oZ5aI34P9dRcSAc6l%2FRAWs%2BdxxziLWw2JjuVCYWONE3GV2eu8XGis6aRe4jRvuaFaz7l1TrQLPJf4aXF%2FdVNK%2F0BE7uZPXaYEjelEJ06CkBM8oC5kMQJN%2BqEkakniNoiVL3VP%2BeicXJwMqqNBy7ZRFdVqOj2IJ6rkwk9cuQkNEjrbwh5G5RM1sjJzPzFai9Ll76zeegcL%2BfuBwqKRKR%2FEZPFP7e5t4PLJZe0nQIELiEFOXl2%2F8wQLvqw11qVI%2FSDldKUBX%2FgNE6ynzkLihjsL2yQDgFH4NNj5opJmoqPwyiV7epogoDD1l%2BjHBjqkARyewTPkSiSf12QxqaV%2BuEBvpBR%2F7GGt824VAxYJOWtsvVhENIPhuqd9u9uLSTt7iy3%2BzJ4jO2DXVR%2BJAbNT%2FDGLPF2c4Q5q91OKZds76CXFBF%2FYGiXW0rhIS3UvVBJik2xmFIvwPCRa1XmL5uEPvGazihLf9wVAGjqorPehXimThDpTn5X%2BkFI9O0HTdJWCs5RJuS%2Fng08hco0bfCokE7i5Lpqu&X-Amz-Signature=fb7c9ecfe9dc2b5ffb5eb55b7cc85ed41c3c6c739fc1f3fd1d614b18d4bf78c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

