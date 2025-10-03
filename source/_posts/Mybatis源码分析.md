---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYIMW5CW%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T060122Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQSJBwMBmj%2FRTTAb2cHShLD74t1byXa1UgmgjUhCFI2QIhAKDf9CHloiDNGSfqT5o%2FQYAPecuTdVoMlKY7mJ0H0QcMKv8DCD4QABoMNjM3NDIzMTgzODA1Igy2e3qgbSHLcIrJV3Qq3AMjP8oWSz%2FX5VZLTixm1YZXU0cWSK4%2F8p7CWSUoYMBZZpTUVWbhKcyBiGPeHr7HTtaNcVHLoBVjyxQrIxCB%2FmGFTcrnMhk8i4tk8EEYf75zoSMqVQYrpeY0juPZK%2Fi%2BH%2BWCt0AGUYfRA06IieUzixIDFcw7wkD0ItahQzunaxQpdUB9f0UKeX9niQvVVLI1NXI294Uxc74KsGLFwdY4bqMuqiWLlvx67oqrwWnvAOcP98UxEAGmFYaA%2F%2Bp32VLqGtBxbbb8oJFxj8Cd5Hkd9MuQq7uJat5EQDT03T2LOKaOVjrgz57LEOugF3j%2FnnYTF79V8zNBaBAP5AVXlUF5RsIm1eWowTbaGzazCDV77h1pYsJLV%2Bug9z7t%2BMYg08qMy0IiYhTTk0Q7dVX1fy9dvQVWSyJPECXFVvaOcEwVM7UdyYw3VmOeNljMCDk%2BmB0dX%2BY5CjEa4Mz1v35xDvJbuDV0paJyx9KWlDJ4hM7BsSVMtIJhIYHayvVY%2B1V0A5zYoZvcKQJMXlz55Oe6hDs%2F24jDD5fgRcv%2FHp1YSvgLcn0IGJ2VW3WlZ%2BreHUTWlzdohzs%2BCgHAPfSVE53BCLUkChLehOYDR8sHWuIFsV%2FWKvfos60IdReymdgEjR7yJDC%2BrP3GBjqkAWoRvBw8PEXfcKYVQhnEkP1TuQhf5da4xytggw69hCJhMDC4lEUkgywy%2Bg3Septdj1BiA8GGQy2AzA%2FwFTzcUEZG%2BmiF96K%2B%2Fb6pWH7jubtElLLB3aIjh6DTLwVzv%2F98d1nikKvdDzrBLNI1UrMdI%2BbhtZFycc3SyoNIZqqyTWcJkYioBlTbV6Q2UJdMJICspfx1i9%2FYlz4nq0zg5JYpurxe0uO1&X-Amz-Signature=2a72636a6867a98140192429648d365f22073f8e76ebda2ab92599bf537b0193&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

