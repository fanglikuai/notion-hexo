---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGD3PEVJ%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIDZ5dyEChOiCpzWLa4t4BKNyi%2FnSMHkgBtnslwJP%2BvZJAiEA5sJ9c7y5Tgk%2Br%2FW3%2Ffksouo%2FqW32580Lpy0ntujvFBUqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEn1xJylnbuQpt9eHCrcA01sWkfH646z0G03o8XQy8UOF1gLX6Sc7p19zZhQMEV0qf%2Fkk5qzGZ%2BD47OlWGQdYL9zZep8o%2BrOCz3yO5LomCFQh2bkMbMfm%2FsRAYmCGaL1KKG%2Bl6l6UFj0NJWZJQBcvcDCqfkOWT6ksOXX710RwmCUQCvgozoG0%2Bf6j2YKgXb2JnuCxT4oIj9LsMaaJvH7gdQrMcVOM5KMUOiAQ4ABEZys3UbQLDnnihWe5mq5TWcWmakKdgm2DZPqgbB68M07sVEmq4AhvlfDjAA5Qva3XDQ%2FWBgY%2F%2F6fCt6RgkCnuueMkBQo65xCrvyAjtZVYjV7MrqrFhVoyE%2BZ2fp%2B97y0ZDmR6%2BYWZXqdpTRuB%2Bw8J%2BT5LKsDlSOPkUxHKb25RC93MKvhNkoNTNYtGXeJoECjtmYV0aJOhmaN3XynWGFYcynQQ7cFSr23%2BXdOaW2BWbE1vteBscX447aOHaqH2D6egJ6PW4DIfE4ig4fc3qJVFL7hQD%2F6kigi9o3WE9WhuiLTzl%2BuvRQ6ipl2sCee271JlCQVZOwNiFL5P9L%2BXHNmgPn0Ko711WfnOV6qlr42zqNVUaIfcrpFjJXCPmctYvr%2FvZetzpN74p38N6%2BFSc%2BevKgPw4%2FelF%2FqhWJ4UhRHMIHRnMcGOqUBEYv%2BmjJwKu%2BvHH7872cSpOPaegED08Mhjve7sY7HS%2FdDZ9K1noz%2Bk%2BsMJzm554Ww4WQEOZq%2BnYupcN1IX%2BGERHpvWyxVSNimWt3B%2Fb%2Bxj4NeF3dZI1gj0i%2BIvEM0SvByIaSexv3%2BRGi1JgXcsPlkZdVOR0XVoyXr75pYgSM15GyIbjgSyAO8mbL58uGP9y%2FPmGDs2rTWm4VFBO6hsodDLHTw717b&X-Amz-Signature=41e510a93cd7b2ec30d95fa9d444fae2d45a7079ee0d077646eb39dbd92182ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

