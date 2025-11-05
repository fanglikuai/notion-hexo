---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TZXVFUW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSNS9w99tJV6pVZh%2FDEEtSAq30bfPFNqHeN15jG1oS8gIhALAZJ3d8PyP8H%2B1H%2BkIqt%2BNhjk5CFbZHHAdQTCqV%2FWs0KogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzLbYBIjPDJuqhWJmkq3AOBYewajdE0yGhChKieUw2oW9biSkkEVrCnj1Tn61AH90uafBTHdGxnY1hSQHmCHxwrmfWnzYUlyJ1oda8bMm7DpeWqidTnLH26%2B3%2BVVyPeApWZ0HDgLzaxRyHdniQ9y2jz8zp%2B5bsAkHcUJHvXF8c%2FwAV09exqBhE2AUJ7K7pzxx9f%2B9nEbcI6bqdK%2BD4RaCAtt6MzWxDOjTkPQFQ5TIgoD11JvakeTwRiaOFKnWW1%2FSyn2otjLfJIGbZXz6%2FHIOXMvFyFto%2BnwwO653BqLQZGs4bWMS2xLwth6tQJ4a4jQRqT4Y9QI%2BR8nljkxei49Gad6%2Bu6YmzUr1Swsa2yqIv3pmDf8yaBpowTfFaPZNtHZeMiRpzM%2BU7k0cwpifuP5LyeTf72cpQPVPm02E%2BABBfB8qMVs8ugQhbE%2BbH2biS9V4dPj2bjp7KqmVXSnSw%2Bj3pUDVu5IV8sz6iIdx7ecZEqXatM2dmdDdu8ncugqtPbEDOzkh9k%2FECo3C%2B3dPoZ39xWxd9AWG7WUyk4TfmS5yy9%2FJzbv07Kwjt1j9SfcLFOIh9cPPdD4Ek3Z58XMezOPhlE97MxIsNUpVEXB1l7E%2FlXJDPefDqaX86rVSACdCRae4I2M%2F%2B22eAs%2FNjmhjCLuK7IBjqkAfWqCZqqDSLYpVCdDkAuvNLQ%2FYjwifrZ4Ojm5kNjYx6LmEbD5pGs%2B4YZnhWaUgAmR99aahZvuhYt6ul5EH4A2vSvkEoVm0Adwa5eZPa4CggR8%2B%2B5GlPpp4kPPLIGpEKkKj27NzQUyayG5V2ey1diIn9sRSOWL0b4IbbVXqBuKuGpjbchoG5uVXANiU1gMDdRd7h47CSZFGJuD0qc7UYQSrS7fXWO&X-Amz-Signature=4511d23c73b4904dfea43a1e05a9cfee3e6c6eb4597e657dadaebf204565a45a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

