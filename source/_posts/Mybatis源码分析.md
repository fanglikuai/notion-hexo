---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665UL4OVX6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNRw6x8bAdVGOkSVak%2F8LGjKzlYvf2sHZGCtsuFXNwaAIhAIBQd1nGQuCW0KuGhLQq4Pr9nF1AwwyiOsshc5tOPh8PKv8DCGEQABoMNjM3NDIzMTgzODA1IgygGmeKWubAdmQxaqoq3ANQrsbnbwPjbdFIqLAc3EKVasVu4tRXgdpDJ0cLoKMDypFzd94osT1VZASE5rAzzDFKL3tYBwQVspmTQb5XJo7MJv62bKEkdHu4gtIbqnvEJPU42lijSi4l9kIGdBgCgv4DFADqEibQFgqveUa%2FnpooBypAscxZ0UEqZk0mpv6z1zF%2FKjsLS35YpttzyyeGFiOnsYSRQmj8VihVdr7I9%2BZGbLU15dxNp%2FR7kiYPQSjZ1XnX9LlLwr6agHSxIhAboMB5FvlCEl8Iw%2BuPVLOuoHh1KIn3M2W%2F4Kz%2Bsb%2BvEGpwyQc7C1X8LDwJaIcdQtGd5i4e38OK8zaYBWQbRozbr%2BfSjdzHACybV4YvxVa%2F%2FDo26l4xMSoFyKHu8ct7MLoZFx%2BJtbi8EdlAQEMuK2O3X8Ekms3NpnCJ1M%2BaQa4Hu7TLI5%2Fqw6VXc4hNa0glIQqpbqBpvH5GCObM40ofK2j5N%2F8H3g1qWfTAB%2Fjuk7R1qOvCVskF6EPDPhJDSaFq6midikimaiKh90BdFG9BGkZKDZ9UtSfhk3xKHRfC%2FDXT6DUxaP5MR0pp3NgqqXe0yM5SpU85OQ%2FTVeEjQBrWn2wxx7sjqQZ2ukFlcfQMg7MaLOTEU6rYRLInTnOoE3lqrzCTkIXHBjqkAavIK4FnUvLyYa4DftLGJJFf4LG%2BQUI%2Bm%2FtSYWCkpnA0ikgoBYKrYCvbL%2BiVtiv4dlpZgSiYUPUMq0U9Qvjuw6lZDg9d4nqtaea2EcmnN6iXvCjHD4CwMkf0%2Bo2Y7UmqbGigRjIFWlIfna43H4OT%2FR4%2B0vCpEfbbVphg4MygIHTHB0qbNV2JHRZodGeTb5HMpuDOqhN0nSsBhI0GFg342n5Zuifh&X-Amz-Signature=a1f0858860cc287e040a6feb213f9a5568dd91bd4b9273d7f074ffafd78d25bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

