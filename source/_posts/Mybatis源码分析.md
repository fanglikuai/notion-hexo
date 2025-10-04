---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSIUNAOG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZxLOcBegfPJMpgeof72Fkas52stQgMoY7znJ0GKbxIAiEAoE2qrUap9%2Bm4MxaX7vfatuC%2FQwW%2BFk5uVyfphMHh3b0q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDOWiCngWLk9zlCu21SrcAzZIb7ZdylPQune4R3qPyBztXupndzxQoa%2FIj7HQSM3fZwzqm4HbwnjFbXPOK1rtE%2FDhGF7PTQ04V4m%2BNlXX93xU9ygyb9ZDOmhBZBycD5TCwd%2BTkY0USVr3rhNp6pgHQfrxuvNh6yVZGgLzo%2FPcr5fvoHqIhRJ2vq0oEZkIzUxj1vgWCVqetmGXE8bWftSyv3IeNu%2B7D85wdHuDp2jrns47KrmDmLJOJHKv9gfaX%2Bg%2FncXr%2B87CVXV1m7QhXx3RYF5XDJHzegRnANXpu8KjsGgWL6gmhk517Z78aCmZ1GK2pmsE8erHFZkWUyMiXMMo%2BVunYx%2Fu%2F0q%2BifLXAEOtdKZKH4IRRHdBa17O9L3Z%2BjBktCVi6Iq0WH%2FIA%2FFnITaMY%2BG0QJOIORmc1272QWHjnYDuxWicPiqKZmEM9Bl1XSeJsXVPkFCB1F7jjCaIBmTLEZyC%2FujdtGuqkTYjm%2BhsJtc7gADk1M73x1DPDub95pXKQpD812esfKDTh5seCR1BgxaSuUWMayPAVmg4mkDcksj94k6ruSYPQSbs%2F1SqFemuWaiHxkT5quHWoTKSFqwo0RA6eDYLw%2Fg5kaPgVDLpbB%2F028JzHdcRHo2aevtMll0jAxaUW6zvDgGFy3uXMNKbg8cGOqUB2dR9WgpWsYaFeMCDNXtJLrb25DLyz7tzJd8Gs5n4MZVIgri7bI44Vhgce9syhjnvpNMvgKKsqu9gkMAk6Q2tAue0qJ6tyZzAVZu95Zy5u8yPCdY%2F7BHf25mu14HL6liZTyO7fuHZhQYf6GWw2UBoKf%2FH45C4T097jXwur4BHvLakMfnPQ5J1MrKcHQRdhcYsb8Mzp2Qi0MMagUI3Iwycce2k4BKG&X-Amz-Signature=ff4fe1152016bb2d59002d5ac570051c7b586d9692c8e1c846a1a8abf4d5b4f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

