---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPIHHDK%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIF2amej4iy7pJBLXqLrVi8xHwdfr2VXgbSfpkU9EgoEWAiEAkHocBYp331yOkel01QVkaRO5KPAgAEiQVJf4COWgVyQqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BJHbJbAKaHEzmdtyrcA%2B9NfcmwjcXbAz%2BMhSFJvCBcaYf7TUU57Iqa6jI81YuHNTwdjcipdVJo071wf%2FZ%2F6ANuP2DTjXjNfT6LIP8c9ay9d5iUKOCN3c00uLl49rn1uz4r%2BFGKp7VOsa4Wi%2FUKUMINzjkeJrvVKW9lV2xvt9jO3x5RMIRi3wJrvzukd33I69gIup9D2AvIZi0HagcxGvNlchZLVnS7AFwz7RWb6pOPqsf%2BfK9BdZdQ7l8641WFyEnhab0TWTQiPB1ZD2RMtKJVVS74GeDkZqDpVn%2FoK46LXXGfgrkwK%2B2zWNficuQIEY0gO1LL8I7%2F%2B0GJEU5o4wnGBAkwjdv79e8TRB2CPViIebtnvCrq7ORMv5BoHo8A%2Bwp2tOj%2BO8J1ACoxwbFmMSxFg3J5%2FFYOYTzAbM9FV4cf%2FBSwg58588tko08Xvkkcv3xrScRfE%2FXS8GHNdMCR3Q3KRc%2FvoCI2pl5cBF2Sq9CkV4R0EMda8bF%2F1i23asletL9CiBDKRpG%2FdQ4NhQsd6bN%2BqgaPF5Mt2WHu8jCFcla9qoMO5up7so%2BdLeGl75XXPprKy958uGhKT4OyKH1Rsvt7fltYrjrzoWOyFDWuiDLEtiOrT73rsQF%2BmRnqHIQhdlecNRgCn9OMkzWOMOyvlscGOqUBUcKB3yNcRJbBCTKcwHBHI9fkq70ks4iTXrPuxNFFqqNJpYdpENlNGhzRPf01%2FbkClV98to3Yrq6xOgWTq1RYJZgvcuNKL4tcOdkxvnj0VXojnAZ%2FWKlCupXGKWrDRobvd7NqNZHBZJPvVQOWwG7c3htnC8MbyMFnpJpBgs46ykMTO5Ny2gkNO1IYArLIJ%2FiUi%2FOp18mx1KLB8igTMpIwDw7dS5bz&X-Amz-Signature=c08cc9d0fe240a60e3f733d423e6cdeb8f78567df72dd38b0ba6eded8483e660&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

