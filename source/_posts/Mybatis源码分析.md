---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2W3XRD4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQCrY%2Faf6nKePgTMMWBEoYICWYAPKKuLEVEunVucmvOK%2FQIgaLzFqSRzo0IJg3nLWlJkZET4G4AM35MaapjEZbakoxoqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFl9GPUQam7rIlAEHSrcAxSZv1Veaj%2FQd4ZGwj1VvZn%2FWT92FG8KvLVP2fTN27BTlcBulMC8X7a1XMlGKBkcZx4bhU7qkgfSy37VC4THiatQaz5a9HL5N%2FwuOay0qHjvfDABBCMoKNbDPtWeatCSB1JrLl4BJJxvaK9Mx1i9Ig6nF1oFy1H%2B%2BvgG5UEm%2BlFCKVpgPNYwOF4EpSh4ibxM05OQQ2n6P62rh6fCrwvD7sTxq%2FeHLweyFZznbm1An9B38p6ZQFZssLqQeCQzuNXZwCLV%2B5ClFZ%2FX49SvroXE1VZ0y3SGnohWtHdWV4jea7aly7MUu%2BY%2BZB85A7wcuiMqkpNAUNsgiHUzvuxhNinRZix7rP0sGHy82UCmXtvNkbSc6HlOKf8zI0KXqwhQAkbXuHG34vPcKK92QkFNC4YxeILh3hwrUByAJQ3fo8DIUGMJ8Ky3PteLK7XYklcGkB4gGW74Np%2FIJZJSGWpZY45lXsuKhch0qSCgBM7JJemVfvo88Ljnq3ngRbdCvGki4a1tCHvoZnE67J3UAkB35bQeEg%2BvfZt9Ke0hQTj5q%2FRwUxRqKc9Z3Lo8X9NkynjNwWmUBYCPWR1cgpZ%2Ba4VKibl9ewAiBe9LXRsZtcNWdw5%2Bj6ce%2BGkKMcSgY42Igj99MLHqucYGOqUBillWEu6%2F1Eyn4hCd84yL0yz%2Fg3CnebXFAWjDrhCRrMwXcfm%2BmNw1FQYK2%2B0W7GeoedhXf8eWrVHm9frB2s8N8uNKw2KkMh3bTN13vfu7ecmhLdedOFFi2NLnvW4I8Lf8yx5QMxmwpET79u%2FjvmAD6GjW9DkpBtrrHE9Y%2BJNI6%2FjkGlnZ1rMY4PDY%2B2ypSK9NhA7DRR%2FSfVuC1jt6C3Cl4I1U7RR9&X-Amz-Signature=a2d232925c3e3d3c13bf87ad10bbc647b16bce36e5a843113631fb75caddca98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

