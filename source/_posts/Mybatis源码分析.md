---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664W6YVFOA%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIFf8MHeRHd0Cg8Yk0BFGXxB18Rc2Ala3kKP5Th2b5yVtAiAa1AKxRTv8ApLKFnugKfTd6YVBPK7zRZrOzl4z03C5ByqIBAi%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpM%2FeC7ZrbkVHexQCKtwDoS5CDfZ2r3ttLaV9aqOvEQ8FEtX16tjDBM6fIe6AxScAgToGBOjFbMP%2F3eF%2BzSfmszerAxYkhCX%2FyzfOORGu5rihRdproxnKoa8IHe53mmG1AGssyTlzeE9xaMVh0nM8qf%2FLbwFH9KqEpOnuQiu3Ro9Y3BClt27m0tQAeVGBPu4JfdePEW52zDoYQgZ%2FqCM%2FsCbYIJavp0lsz09Oy86uh40juHGbJzE9zuqVuMUd3Sz0e77tebBa8QnAdoTPXMqPN2YXiJDH3571GbbgDWhoJ9zP5VO02YHHnvm0AeHcNPtccnzPKH4%2BE0A2OQf4mmveXno6KQp9ZALWh3wBn4BTGnYprCzR9P1WPMxMfv6Y285fKLpjxt7VdvgMRwkLPJZK%2BSkS2aN3H8JRMyzq6MQenRZ4KCAerHUtgW3Ix8pm%2BAilypt6ow1kSlG3dMRP4AegB6TFn00%2FkehE4Yd77Q%2BoZJGfbZAJ1cKSG47PbKmGCb06L5PJ%2BJSrO%2FSp8k16lBpzoTxtPF2U7RM%2BDGsa8Pu3xVDNnz6RgEmOmQUfqCNHJd7nvJfCap3HY0QWapX8mIxh1s%2FAs0MMlVJLmxbtJFrGPbeJWno9hhDWS0CmvQs8Q8RQ1canMpvREXmE668wjYWDyAY6pgEsav7ObyU0y3kXlRpYxVxSaLb3ndwqDHdfjoJ9BwPuQC49rtFVCjZH9bN3f8r9U4QqRioEM%2BY0Fy5Fshpw1a9f3kCYB2ubFO5t7Xp6L4vr5mJP5yMS4cGqzyS85aBZIA9NC9G%2FiZBZpyklt2wcm%2BeTpgbWh1NKYFblWlQpKbLERuesVWxXxVIvbpwIXrkhmUxDdsuXAR3m5VfRRUQGr1DXD0WiV%2Bkg&X-Amz-Signature=1b7aadf7575ecfc202165b74a6d7298cdde6f004f69e0ff67f88324ece8194b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

