---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYRQEJEP%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T090106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQDtMoZYJh6%2B8Xe20FQm3%2FitAmpPJYguYbAT7ygjJnVsXQIhAPx0FESnemgeSBOf21kE6Kj4oV%2BO8JnccA%2FfTZ2X7Q1vKv8DCCoQABoMNjM3NDIzMTgzODA1IgyLrmfv7O7UXy%2BqsTAq3AO%2FQngz6N%2FUe7fAu6AQdvvz3NIfjmztpQQdvyU5VMspBEZ5c0hX6KpHIHBApMcscDq%2Fny8Pcxen3ZFz%2BcLSXo6%2FuNbdiUOz%2F4dqcEIL9qDhhi3VTBELz63sYK7xC%2B1XoATz5SL7aXY27U3SndT075A%2BZiiDli3%2F%2B68OMA2ZDnpeyho7oivfpPmDpfoDnRKIctYqg4TYcb0VfkTBcCk46n7luwj6o55giZmYDrFBjlx99Nv8Vh9wkURVVT0SJpzqA8UQ4z2Lk%2F9QRPXUibCXfnnXg2Fpy94hsQ%2BhFc5xBgWuw%2BQiARRIwfUDU%2FXGoRnqcJIi%2FxxshS5LvxjYjZ%2BriKqittmHRnrM77lsKAwksfo4cX7BIvekzz%2FhwW%2FSKGwPeag40lFyzq4GFiaEBqJUk9%2FU%2F1dcZaonuwGWSlT792E9%2FM8VPmq3pk3OevUU2Yy7ZXxiUuWldU2AsdUf4RLnEqYs1NYwePuRMFzVnL%2B4IID%2F632S3kXqRefR8XfIX6XmToOFG5m%2FlNc%2FxBdfGioORkb6Sz2RBehQhfvMxijAvBkFHKqShGfsJGpHqs6UeUnhBYPSDod4%2BBWjEEkXasT9d3bcNuAQ3xtkskWzvVqVurFCqK%2F7gGL92PtKUMQgZzCfuOLHBjqkAVLzIPHLVlItXuhRDhy47EABnfn2wCQRjWTe9VLLIoonFjCU30jKm%2BSyj8%2B0E3HcUaVsXMdUPRCvqHDox7kYEgA1IdQ6zaCiogHtiDOJw1uFC78dywE6GKU50iMGVUyNQLOMJbtlVTBw60X4gBSuuOIWfLdsnkmjJ3qCAzRauQQHm7ae9thLKvnd1wpv1gapzeR6QsMn27oL3AIMZXL9acH2TPg0&X-Amz-Signature=b295393b4742056a21da16be138bc9ec242640c77f793561ee6e1599e0901afc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

