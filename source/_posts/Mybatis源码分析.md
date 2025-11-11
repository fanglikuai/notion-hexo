---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YI5DELET%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQC4Li5frRcxgz9BCEPo52qKc%2Bmy2AVzPV7iilFtjT9EPAIhAPvYV%2FYvElmUY7REBgK7W34ys6qnNQmHQEy04Pdb6r2xKv8DCBIQABoMNjM3NDIzMTgzODA1IgxHOe7qF%2BhtoazcEgoq3AO3xB%2BXfsT0GuYKP8SVvDzFKQ5iQ6WbhMDl96dgG5aRo8MtjBNvPltH36kKqRRjJcum6BOXKyamSlCD1UWZeLEo%2Bi9%2F6jmJyOL6jPBb5O2oadZFx8fyZpMyxAQUCKpK1Ab8fqfE1bcUYugptPdUq6HigRQN6860Rb0CsgiX3pNvscKO1UiTTwjGVUkaOoJJ7bgW7WaKQI2%2BrrVH4BdN2z6eDj6jfOQSD8v8weBScdTJS0RlBx3C6jIJO0tcQOyAVWE31G%2FarSmCzetmVWcqoAwEdrrnV813hODlWqdquy%2BUB%2BLFUVA%2BBap8RG3qUOzC1ZLxNOYvlAnVpk3Ku9Hhz%2BxHn3Wwhcweebs5VqjhVrKR49A8gozkvUTE9Q3K%2FEcEybGhGVouKpSOR314%2FY%2BJdB0rXp4U3q9aKw1sXnNz%2Fxo3pmF1o0qGiMYWdqSLlMZ%2BAhtMsVkvraN9i6BrvRrYi3a8gBCQVHEQc6CY2AvlSReHDKuow8mCFgAG%2FRZ%2FrLmOJK%2FxvvIbwox2DjS2inu0XUwiGRGfzg%2Bk0BKPsckuvQT%2FhapkScXQjl73g%2FxDprx5Uso%2BQk2%2F02Bf%2FSPg4zgVr4JKmwZPwO4bT%2Fm2HpKQDDgSqyxPCsoEH0FBAZv39zC2h8rIBjqkAdDUzgMXwpRaKkjdxj74CN4O8Bs6sDn2l5M1xNJ9L9DKSAggh%2Bniuk26VFzCC6pXn%2FKSgEnXzfArsvqu8fdQTygt7CPtSLKtv8WSZBO4E4H6AZSeK8k2RxKsk6uZ%2FajYYt40SREHJvMc1T4mOp9IRWwJU55WR5m6MEjtnt4teCb1LHlEhBmHuaqE%2Fw5ACYL04iient19DZ4c9YSTKX81WkryHMcJ&X-Amz-Signature=5c9d1f645a7c37e3fb9f18e6ed3443e9ca9086cb135dea89bf29aa1fbf0fbc17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

