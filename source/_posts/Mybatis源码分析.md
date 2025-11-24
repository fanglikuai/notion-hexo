---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXESOIZM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T180230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbrQSYlZE6%2F7rE%2Bj08JG4WRnd6Sv22NxxxcUtAknx5DAIhAIKDD7EQhtoKcCGDQWI6W6clO0%2F%2BVQc1ga4SS02hdOFFKv8DCFsQABoMNjM3NDIzMTgzODA1Igz3luFQ9LdudEi8pJIq3AMjSp7v0tk4%2Fe0onE9NpHSXPnpkWbIHLZs44g6Mu2M%2BCfq6SF%2B3LRT625JGdVRATTC6Mo%2B5%2FNUtBMA%2FScGIAHbjne7tuAHWYvQ9luuyECEpDMlNFIkQBocyqgswENSw0tkFOKwWpspnTu186%2FXlla8KSZbyt9z4QVbYZI8QVVq6ZtFZmytZPLBdpfx25VGnMgJtR4LlSiLQfC89tad1iVPdfsU7PTLCoa0yq3Rl1ueer7KyIp2fIyBuTDlIhFt3RGAhMNC8un1W9slQUnmPrYzvwyoPyqhmn51o8UCBknRjHaow5UQqbzN6tm59Prd6rWRnH1SJCavH1FzqHkMO2czpUCmf7I%2BTUlffO8bD2y2ZZbU65MaNx5yQU7ev2cuufIBVbBWCB9rBST%2BqA9ZZJSuJbVk13SJ%2F7EuUuurIrpQ%2BKBDqPg5PUg413lqi6XMyur93yyx87v65IdxY2PUlP%2BpJwIgF6bNprTn%2BC6rY19gwS%2BHK%2BRc5v60r8RGCa9S3Ut058dJzH1ctrgPmfN4boslenaTXeuLmBw%2BzKY4Bqj4O4VluHf%2BaGReQiCfu7YKPUewxpv2ts2fKEnjtA8PkzqFINEnHYGZ9amIsRdYNj%2BDa4vbSR87aRh%2Bu8yj%2BHjCkupLJBjqkAf1zT7JuIYclEINugl14Kx%2FffjPf%2BojbmIH9V0o41BsoDzIkUJRrbdUmraSQD7geQpxY4VDT649AANrZZq7xjIiYi0xN6cZdRhfEoFzc%2FR34skChWycukljlwNTO2ltzxwfBENeniKFf%2FF8tPhNPR2N22CEcNM02ecyBeJ%2B7XnC%2BPwBi7tQ9hFUaXDCd9OyD42AoUTYY4ZgHUq%2FKe0kJM6fxhxzc&X-Amz-Signature=dbea7dcedf750da0f8e958f7bab137e2448a9893b3853da5cf40c0355ed73bf3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

