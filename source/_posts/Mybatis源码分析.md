---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SW5QCB5%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDoz9aWvnLCP2e3eqbz6agMVyQz0%2FA3%2Bk6EF0sZMv3hXwIgSnUcczFfEG%2F9xySepsQcHyBzZ9F2cx%2F3nXcBFlEp7m8qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcDC0v84M48dQBmKircAxijXnJKBrPn6KUpl4r1eZ%2FWzcK%2FPJh2KvzhwJKOqb6gFARuHLDNrRxf7MFfuCynBflD1H7o1sGRsGQiTNVB2T3e%2FTr1VahZNTuHaTIqbey7vOWiWB5waZ7VM6%2F4Yd9Kur%2FY4FuSAu5W%2FaXTYWDfWxds%2F%2BHnWDHDcXw%2FjknKm0peXhzpyZUpVYJHEJ9s3BMrA13ouZ09aRhouXT5p7niQXZNR5Pfiv4qtr07ir2OBeNRqUAKNrQyp3MZNc3cqzf2Sl9i3fVR51ZzD8HfZOMeauXXlo4vX8rWC1rihrpjGG7dZD%2ButawgDygOvGO0zmnFC9lZZgi2AmnhFdqnaXp4o6iuiFZQcxDadiQtPs33DNED4f6w6cPNxix2esq00cS5PBEiLYgtMAgk7xVOo15k8Z9u%2FEhNPFOltwK1DMoXEmIDFeMV0cuvfaQadoqCPgiyhbfcG03MSrDgoVUCGgnoKafwPWooWtP5uMoU3mijRYhInTilA9J7tKx2fPzpVqgnzpd6NaoINaTrYAYwhCwqfF4AO%2B1DbxhvwKYb7Q2ncFbRQE36C1nGgNZquxnvMhHpyOl2IGc4F%2BuozQGEs2mjDq4%2Fbipfk0fuBdm56vMasIA%2BblXqeVvSD1tGFzfMMOKYrcYGOqUB%2FUA8mTBtd6muXL7UooEJE4r12BFKTeUhFJzMY%2Fae%2BaoeacRLQ3ER7ZEZDk%2FF47ZcaGBeWwxEIVhsVH9qx2cSh4E0eRSaJnw1g08t6VZE9KDRQte2de9OR5jij266Eq4FugxYt%2FqIDOKWzTwCfhG4RCaNe7WIweIbQuIevP1iRHuusUJcuruLAxNwQsjC%2BPC6JVXOs8dgGO%2BiTt5kKDjp3F%2BlTKHo&X-Amz-Signature=0d39bfefdffc154309aebeb731975fe128b04972bcb20564528e8c2d77f84a90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

