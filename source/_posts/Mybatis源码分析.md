---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBQRPIEE%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDLzo1jqRfP1uBl6nodE%2B%2F4GqoHqkcCxh47764Rtl8VIQIhANV0SsFyovG4%2FDNQ9cfT4poVnJk3jt7chwehaQKpj%2BJ2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyIPWS%2BeKOgQpXoZ7Yq3APTCp0JxAmVF2%2B5kWr8NIGCMXBMORWwxFcrqLt5GbNWlr%2B2%2Fys2hGr0rx0ZVcBAWzcpLkU2yPgdVqJ237FE59kVLwp4RD1mK8iGUQHaUfGLffuh22yefZX1EAk%2BP97KjYXGDP8i99Cfp3l8oM8TNNLWTJF6PRVO6OnlM1AmDmqzkwQBoHbSz2UriZ%2BYI2fhkz%2FLnIjgoEwfDYIghXVPIbHshVCprkL1yABZz6IVU4pEVM7IZNvDr3soiiL%2FDeNttL%2B9o6Tj1aaTvYpyRg0L2DzzNrQegctQnJqcu9nDWzdiw7KGXHJeYlETOQlx3jQcVUPz%2FDuKuAkvEw9Qz%2BCkIGm8VtItmI4XWdcpL211dRKJR7R%2B4u9QWldXGZ9DG5dbIoiV4T5zVaQ5ED%2FcaH%2BjIKTF8rM6rsQLsSDRmIHGUx85p26AmRfoc31Dx2mifnuKJatrLVCTdJnldB1Lq87e9%2BS67y8WJezOWKCh3lRK9c1Aacae0WQvNIWW0MY9i1f2VWidaPlXVHLK%2BQLAiiQtvmoKb3vWYw1lJ%2BNMjTfDJoH4wEIFtStVVgWhn7PDzNGC87m%2FWuxnwSHFRU7iKCMWyvZ%2BKXXHjHSY8qnsPn8m7Ki4qLYX6bvTCBpisNAwSTCDmrLGBjqkASeFHwdqnamOfInDrCyYQUHCyJBAl5009GZxCGW8UJiIg7KqOXB77GIgio8chhNbTpGEggRHAAgtzvNRcfhBvJ8dNELllrndmgN883b7qskYETIgHMIzcJMJdZ0nqviuKyFHfKYITXChsslsk9lNI%2FnwNEE%2BP5Z2vBClXaPakpLQTbgkBX5CsgnSCPMJ%2F25DnfNOYK5BHZIJV3Bn3IyycK3%2Fw4wW&X-Amz-Signature=db07ccc47a655d8a1f23c15ebc7eaebc21c3e667fd715aa049067ba0c5c9c3ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

