---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632E5UWOQ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC8u1pi4EXMJVu%2FpCqbfUkh%2BeZSKcj5qZ%2BGc5CzlYVh4AiBzmpayOdKQP2yGjgfVGLWAgPyKpwHnVuZOXoB4BsasXSr%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMUA4M2GKqvp3JhXCZKtwDtZmuZZX%2BjZJyezPKCDcNjtPe%2Fb2dfDusbm5AUpHokfUMDDVwmFzX0nAfbAGy4s36B%2B1AD899ltbK1905B8CXyNEyvIgT0wqsjYuqLWvDpwozOrNAJ%2FxASwyU891dS3P2ZoERPeH2OllmWiF63dJOXibxLneu3YGFRT3ZFx6QaEnX89eE1T9PIU9AMBuoHX5g0gFnYW4bz6n0Ogi1izQrsIc3%2FeE5wi7CNXspTQww0pf%2Fgwny0%2FdQ743nqbeSBh8NfM96Lp9NaHy4JF6%2Bw2F2kwY0mrylrCacWi9hLS1F1HPwOgfk2oQy2%2BLeKgFKSfwxaaLi%2Fpls6ihFvk4TvhmdfDTIM6cEONHUUdX4iCHuVxrIQbND8Z6YookZrTntr3EU9v3eQ%2FHZwhZjKANlNz3jc9Nd%2FFE4FuNPg3T0BW7bsKQGkaNJZ9dPXC39p2kzMWNqj4oMSp%2FV8u%2F9jz%2Fl8yLLaQB2L%2BeYsssxE2y9zlY7h%2Bnj1JT7UpROvVfwq543lFkfLz5e0hYlxkseKl0V%2F0t%2FjV%2Buw0etHJRF8Z7lhvqOP%2FiO0fUHQodGFsh6goGfXMLEGA6rqp2paJRjWxchakzDn%2BdHZtnpguIw2hDT4cqbTieC0ta91FT6sUuxehww5%2BXgyAY6pgFhaPIFCFwdQVaMoldCOGeoYvq8EggfWAczzL9%2FaX5IwfqH1XxGCiDYgDUj%2FIgH%2BHA%2FwSV%2Fceht29UKYZaRIcwEOHHoBfCuRn3WWs1ZhAUfRv%2B8aBrLPEOFo4eu0e5JLSRP3h8JdJXmvNuyahSh6mdui3QFGbrKEr2oafJzr%2FYo5HujL0zXcK5K6SintQLferPtW5PY22szI3QmNt869bcXU1zXznqp&X-Amz-Signature=0220994259b2a3ab717d046944cc23932f031c142f97cea0e38f89ff7742b5f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

