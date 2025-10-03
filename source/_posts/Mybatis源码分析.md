---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTLAAY3L%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC0CKMGOy2oCdC165xfiXfkBwkrQHoVBPbSpY%2B%2B%2BO%2FmhAIgSHOVhDZ9BcHcpbB6CidwKcVbyohCcCsAwbeQ%2BOHc1%2F0q%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDGEgaYZl%2FVK%2BDD1YySrcAwzj7CGFhO5WK2pmUmi1Jla646MFegnGZnSA%2Fywoh%2F6mvbWpuJwh2QGM6cMk107YL3GCSKVjDTTtBcD2GNvbQ6NtPb4Hh%2F9SKA1t9aTlN9W7ileGuaNLBRS%2FwgdVi%2FqjIwTglpclWUJIieXoTmyuUyO1nmig2RtD6a7Og2uHnydG3Ddw4Q2iDI3xhNba4qyWD8QWQlkjNsjOvM%2BhKqUZI%2Fml3kpPYLJVjqdZcynJGkb69i6fCxjSsIPnSBNrIqt6YZFacWM0ScE2QNeYoxKzYX%2F47ghitVcJ8FD%2Bq7Gb43mKSsCuy4zHrU49IYxffLpYPXeH0xxP1Yn5JaR3k%2Fabdw1SPd1yIaWoaE0sZFLUbJrpjwI5PdapbTo3ExpcbR9iPlQghiOy7rt1RoWvQhN2doaU22pWKmgJrMEEKlvl29lPJ%2FF6W%2BKEc49%2BONuzP%2BOPKFixp8qkpOCyJ20Lq4fE5up3a3qz0iowNapjoySExrBdDZytSeEYd6VT9PPWF9yA7SS5uPGuHm5fekH%2BR3TD2nRIbKuZZF6HcZgzU4bklVweaw8sKAUmf2uS0Gz6mALl08Jlx9Ijdf8IqF9xXGtN1yj7q%2B%2BEbkfp%2BFLXkMvON42Fzl070DEUa9fZROZMMJnagMcGOqUBAgU4lqX5ZUDvPTponGdnX2LHXThaYt%2BVMJN2ePos1LvCyfmwqd4eUW69y6oMQEfrBUnL3pnbIyKIiW4rFlS4Szi2HSuItlJtmnbs4FgCSehlH%2F3%2FSM6xkhi6D5iRXg%2F3uPHD1OmoNL1hWMPc0IxK5VlIBEha0OvOAfEUEzsni5Krwk50zj%2BZPpEseQfsXLuJIdhy5nECSbT62dQDq87QTRBf7K9o&X-Amz-Signature=c012aa8eb46b369c97a0e212026cb1144a8e7524f6243540669791065f83653e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

