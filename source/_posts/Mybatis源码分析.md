---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622DZSCJR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIFILltTsQMdHonUYygXtuG%2FcYGqhZtaZMrKI0tToTKLtAiEA0%2FtODN0AnxIpC9wq6Aiy9ij%2FQ0ew5uZc0v%2B0lsnuxZsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDO1PTwXU%2B%2FaHnKsWfCrcA9xpDb20sKiIs3uEVp6Mc5bO%2F6mLIrWGZ193EKsY8Kx58CyWYpbN4V6cpCGIpOZw5FhwmjudMc6Fa0YLwFivYdaVwcGKWUHf8r1VJgbw%2Fp3qxLMoEz%2FPMTcpuO22dCrLF5W8Bb6ixX9sXQnhFZA6NlODejicazpmDSQ6ufu7xYyg8a07E0efkn0NfMIrGr0Dy4wM988uHEPcwEUAWZ9QtAGRB0IIyFhX5OyhnIHOvySr3MNwwuk%2BvGnPfFHx2gpAzdtM6J3xkUfFJthxqkrRBmZeI6BMhhXvbMqPqjUrahKd9DEs7b5uoTXkucKXqFChnl6ghpYEaCGzJCqLkd6EhQy0aO1pp6Ox5KzPB3mzjAHuR%2FsDaWpJ7ZhR6Bsr7NJdNdGBNGWC7OQHCWU9Iso%2BwQ6fVqvMkaB9%2F%2FlffyCHPoTEN%2BFySMocG4t8VJSabdtiFSnU1Ly5RATvaOkZ6KnyjeKRm2yz%2BDAYh%2FcIMaHPOtIk8tyWW8VasOJZd99%2BW7T%2BGceNyuieQ07zrQVph6H8p%2BSxcx4OT4C0vEa2FCTmzbYS5OW5lu%2BIUj4wBrt9el2WWgbpOfiIwM%2BFAE1yzoSXF6QCE1MaHZHov76PYhjwtd9yS7%2FKmq82Dlw%2BLyOSMMCtlsgGOqUBuUSdeK97xXQnvDrT2kDKZQlAGpPe%2FM5lPTq4PFD%2BbEfQjfJDQHsFi%2FifKP2BNXNZVOm3UTuWx%2B5qHSUxCwetR9uTza1A59POTga6nUpawEZM1JK5KzNua8X%2FQay1T%2BsnNc5vIdGeM85y4BnYIVsTGTVHYrzuSgC%2FT%2FU%2FqsolteqqYoaVB55xUmpJLZeUsGOVcyiUargCQrpGiI7DDVGD7Xy94y1b&X-Amz-Signature=27c6ad8b8b8697924182bf5ff2c6d1bebf1b35f243df7948f53236680ec54448&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

