---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZJUOTN%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T000036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvQQ4o8N805BqL1GhU2LrvMd%2FPt1Jo0Ry4sMUKp%2FgD0gIgSgmWlE9fNVn%2FcgO7iz27ExB6wRjQcqektJuGlPO1fskq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FfwBggwz%2B7GgKfEyrcA29E08SnPCJYDc5SHn%2FwyBczUxRBfzst%2Bz4BC6KgTrn%2B8nR3J5CiUEy3BSJD8Fb0Z4HMIh4w0aQi7SxZNej1dYOWvVlHoYt1aGMTH2r%2FNdAKJreYI9DfkeA6IJLROF1LlzpYQGtT6%2Fln7JxXRFNi1cePDGnMG3NOeAlot9%2BcNRifEOEh1NiZJ6E0b8HSo42ygbm7zPfZUKuehRyc8WlPnjHGASJ8gvg5UoXFrGYZjUGTWNGVHsJFcf5rN7vk4US3nl4RRGcwxVbVUBX1Esw01dkZovgVcn1%2BHsGezRXKqRFnGyl%2F45CKYmoquS3Eh6o1R9IdwwwH6Tn2%2BbK8CQBoATk6DsXq4%2FG2WAUX22GYSq%2B3eGjBlfLoO3rxbBo5uJQG9hUb2oEUQ4AHaoKmeJ48lSTnybta4R50niNokL3mO0bNCWjGJJWhXmnfYYNeyXRPdVTV3%2Fu4dLuMlfuj00fwPB%2FczkRJZnJ3ogxi%2FgpfmtwcMsDoyxW5i86BjjJgDbOTolCERRlJ1A%2Bh5VTJYaaeKs9h07b3Iwb5QucbSsNetRmeh4YO8d3rO%2FDPfsbcodDoE7%2FZm5I6G2pQEDbqeiJuo9v%2FCM%2FNw3LsqM5bBm%2Bp1uFrcWz8wbqVLgCuDIgyMK%2FhhscGOqUBH0qTMoNCJLt4%2B22qfiwIaj0ZBV4B3m2VfS4BLeB3ttzrIgCYSCXV7va64H3hPX%2Baudt5URdbtLHczf%2Bma2%2B%2FaQNruPHBBrbEVx1Y62RwBi%2BDFoEIoOuv%2BiSOF%2FjkhymXfaUbGvz%2B1oGyvvLG434CKKdeNFAn9HtSVKhAyulpaNX1vAjSpwOfPpI4zs8qXgyewImxYpmdx%2FVORqy9sYHIvZAvN0iF&X-Amz-Signature=0d3e9d4de96d0e85485953c783cedba26fe2d9e7272175aa51c438f8fa1b19da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

