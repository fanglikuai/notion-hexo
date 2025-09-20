---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIXNGJOJ%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIGl869kOcyKauzSHBAML7ox%2BcZnU6Ovu39gedKUUzftAAiEAhw3SNnuyDr0YCfpp8pP9fiJGsfqDTAZZn4QZjhBzC%2B0qiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBRR8Bxz%2Bjbak4L65SrcAy4A7KjWYn685bc6DgEjAXT9uesFHwLW8izqdFbE1FeZ64K51bjScukRzXx%2F59%2BgW9x9NzPB9y7ZSWC87449q%2FFD64Q2dxu9WjK8LVL0NxLsnWmc8cVa4uX1oKwHcYwDO1NyMevyCnzkeF0OFqOpL0%2F385bT7N6x%2FKdWpTiGo77FzToiYIbgpMtO151mthb03FmsbBOA5SeGrjUcxMIvlYaB2DQ0RgdkibWqs4%2FBeMZkCGCadLyAPhrlFUV5SDWwXykwuGw7eRlqBdibPbzSxrCchzoGjpqzMafbVulmuJwVO7xAX7xdLjCTqYZ3GCmW3GRcnxD6K1p7ae6xbFFiGfDG7qZmbMbSnGuJBZVIFk5imtfeQrdGqddnBQBtA7eR%2B7Po4u9%2B832OJXzd81IYORM39Uvv1qLQutDnmkCSBmRRPO2iuFwMNpvLsK8needuL3MFHHIJETE7tEoXKE2DR7LgV2pRNV8cpEO4Ios8QJK0%2F5N8bun3077vb6FSjKflF%2FCBXtPVYZZtM1W9AESrWLYihDWaFcTNBVxJbru3b%2FCoIE51t8wd8VQIeYRn9SJ7Z62q%2FBFN7pbh5N7h%2F9f79xWFHOcVa69mJnxJw2GjmMs40PYzp5oclvAWqftwMIOUvMYGOqUB4n8YSOX%2Fa%2FQ8468VlFwSehCA%2FZWwrnDLpFxIPB%2Bb2Ur5%2FrDG8kRrgC638f67kMfYKIhvFMg7XL%2BMxrfCdY%2BROIKBKWt1DOsfRUCnjiwzq3nPvHgd%2F8xMtjqlW0H31VLOfS431ABENKLueRI9Y6hzM574oyfXRTnE5tG9fochUd3AUtS7svVs9MUEX66L11vDkPCzZb7bTaiF2OIbbPsprc6Clc6b&X-Amz-Signature=4c5f2c470bd9024c1bb9aff6cbb409315c1d5455a3a10b02d8568879862377bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

