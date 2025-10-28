---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYX2OMR4%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDU1%2FJlZbuIBeR%2BfObX8%2F5p9HAk5UYQZSleVqOJLanY4AiAy9h7KXGgGPu0hbQ9FsGnzLFMhJTMRn5ZsR5%2BCubJHhyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMgNsFuKeTlZmglRKtwDvHbMUfO8F95a3OhfxnLKMsW3ySJnNckNJ9peU6%2BxvTvE10IoC6GRGIrTfwHi1X28u3WWB8rQ91UsVH3lt21hTtB5vKSHcG8Xk2S6p1U8c0VqfOAuVwLt6JCUSSSlhhFJkrjlrBC8kqy3epFOfC7AVRiMy97FQ3sTqpbQUPLCMsq5pGbxWRquNMnd3B0yfubLh3yCpLGw8Ta9TXyLhLNTVYjET4f7vcziFSAEeH%2B%2ButSrk6ocBFWJuWhRp7Ka1LvOLBsWDv3BHlt81IAnrsZFTCv1dWm%2FynEDTYkRYvKgJVSFF0HIhrASkbTmJqiOKRD3Z0NB4Y%2BcVA8hQq9ZeR6xsAXnqlMZYy9x%2BjUwII4sXdrahNFeZtx3UWiD5eQUHgPN5Rf%2BgLnhCFrbQ5LH7h4aXKvHrxA7q2tvm42E1qlqCyT4WjQYKz85mTIK6su0zjTi0PDBIsK3T%2BVBbnlef19%2BYbOcjm6%2FYPr3%2BfCdvtuGvycNzOBPLRP1sEyGgdwoEkiGMo86uTCRTIRFrtfNjOr8uFrIRDYq9ySC5%2Bq%2Bu%2BFVeQWX7CQE7ilOZKYs2nGv21lrNnvuPhYVqX3BpsLOzH%2F6lEJsYw8nPbQAZoTYnTpRWSzZrHNw42b6q%2BTbpi4w%2B5uAyAY6pgEcjWrsxUpxhqd%2FnL2YLiB4CEM0FqIK0iUjELrX%2B8n3D7orh%2BmONdieu0pAOcFHhG63Ga8AvTCpIoIxIHGvHems4g2TfSBOEe%2FWriQNQ1GK7VajKRvaQZ9MSE2SuGIWLg9%2BQ2YglVmeK44l9hHTH76sYpZ50km7LKB675o%2BifLzhNcKvYHTy5y0cgGI2mZ7WahsmNFAX0dYATYblz1NmVo0bNCTSSr6&X-Amz-Signature=1778c3376d67db73a2f6996b3eb4586bc28d866aec56b2867fc1eed6e199ee52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

