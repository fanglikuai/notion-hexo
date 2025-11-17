---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QT3LWVBR%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7PYnf28qqrdAisqC30qQ8gdHxMx3m1vevZKT%2BDAXf6wIhAJfmhKoy3IWG0w02g3rIlZO1kk78Hryr6Lc3pp0P7e44KogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyMtZXREh99Toi0NHkq3APSjNOyf6AYtph%2BAE6t%2F%2F%2B2i3AH4zfuOqrLVbN%2BoYvvS8CKmIKZzVshUfADZhwXxV8b%2B0W2SIeAMlaQW9rnVMGbZ6%2Bid8SX81bYSYOSbEsnH6ce6x9h6RvYwZSn3W4DsC3B1JeZIqaqbZ%2B6fYuu5%2B6Imz%2BIQRmvXe9R4cPPfVobnlueakA8kXKC4MTz9IZLpWZDHe%2B%2Bgu%2F4GQMiUMIq6GsIOwb1bKW9q9NQUSBu3fEj1fxcDIkVLi%2FPHEQM4tl5kAXWJhyejI3w%2B7rc3wDbmQLkytPrPJ%2BTijPRK1urGqi5VTGP1K9uFFIdMk%2BKtbsK0Z2nFqBmfY8KkOn5GuQ7Y%2BHGX2ozlo2e%2Fy2ABMa7MgnCwZ3byTJ07UJy%2FbnrIzzUoFcLEv09KdV%2FmT12uTREOQUOkLwjKTF8AahYW8gi64rb4H0T0pAYKKGOMlq4PsshI4xxolrqU6YAWxgcM5DRh8eXMUWrmPIV3BDKp%2BgNpyRm3KH5bLpMWUPDYeLIACk5MihY0trJsB0BgpfhapcfHARUtfXWnUzMZwhJAtYWJ0lOPzXTNX7JcVLrjMMiDM81oFviJC3DbBjftJCKUSO55ewc8xZ8ZBcuFVc5gVqcpBKnRXgN9ysnnmhqGy5QZzCUwerIBjqkAf98KZkR4fLpJEL4P4M68PKnz9wT4qiDd7vdylQFspC4S8FT%2F5zX5JhN5YJ7y%2FV4CwFXZKida0ZCT3JDT4CCL01Fts%2BdFUgVM2RVKVmTIHUJEpQCXNBmF8Lrgk34HeZpII%2Fz8LmAQpkZQKmfjWKbwixYITo%2BftVEw7LBtOHnRI0QfcEZ%2FN4lvuCnZKOQ2Naw0OihmAF5I08nWTodJ8aYE0MgxHoO&X-Amz-Signature=3b45e1d703f4002ca3171b0d6904d8555cc7f56c560ea65b7e2dfdbe3b33733a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

