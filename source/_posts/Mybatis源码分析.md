---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NYNPVCI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHYMJINH5NVIY%2BoUIZiRl5UnM%2FdlVH4t2kd1QuiBGE3oAiBApNgZi0tLZY%2BeUd9lJOHCWU6%2FeySKZNJvsAE2yKRMLSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2vYbYW6QAn1y8xzDKtwDenZMwk3eARnt%2BLVU%2FBNpqADAJy7TRtk0slvp3MKjblFPI62Nzo9usKIDAz5UlDDh06A%2B0avILaYHDES6omjFaScVSleJzoHUSLJtPn4gKRrHnKGA%2Br0DYf3A8Y3%2FRnKYQNmNEnnglQ8CeQ0G7GMRiiahN9dRGxqsxbxCg8tEgIcpcCQDwqoguw51Y2hP5R01b%2FgO9a2FOIxFHFud%2BPVDcWf21uWWx9dogsOTUBYOnxSBA%2BN1azn7jzLmkFXBCmGR24MaGCQgpL6EU34fOTD%2F5xOiy2KmDR9aP5rYYc3uoU5p3wZ7XVLJjWbqu8R2kmzwlrmY%2FG6DnG%2BG%2B9bhRqMud1p%2FiJ0Pbxm7cWr5xpdUGgJIunRV6jo%2Ftow2%2B1xFpGxsvqG48rQ4oKsDFrt%2BKRnzLI9NJoIU%2BeeGUpGpj6K05amtLxEZxu99JFo%2Bqj7CM%2BeNoq1V9VRiU9a%2Bb7TtZsk5JLY6Ohn5ajqgkYw5U9c6fTU0I%2Bnij0Rzsdih7SJyvErlqwfJWjced5%2Bkhw3zp%2BGXiTxAk2joGKcNI%2B57QhOPp4BDECAoSY8DCwXDnaet8%2BTqdX%2FAHcrJQ69EevWSk7IaiQf8Q9HNhsozVNhywu75rtJI%2BGJFoACCt7X4aJIw2tbUxwY6pgFXsg9RlG0U3xF9fnBJ0Ph%2BchKkK0jZ5P%2BhG%2FNyDt0MpWYuMssakX9j2Cw1YnkoHPaU0vC6LeOGCIykCI%2BToN3RgaLuo9Paq9C7Ne0wRE1BEnlIg1WK9fa%2B7LMN7YqnSGbLn4lGaB1VKEEMaPd8aoz%2FUGBxhquDsLebHedFSrNfw%2BVaZIffcyuJ3tD46t6DDVcZb15sxu1ALDucYu3LxVfJaDA%2FAKd0&X-Amz-Signature=dab52a874f1f0bf3a629b46aeb3e0a19e8440cc2751d9c00b878c1b812d42923&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

