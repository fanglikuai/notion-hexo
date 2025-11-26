---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626WQUO3V%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDJhZdrpbSVrlC9T%2BuilwkFo2CenQlVR7VGmJX11gQb2AiEAvxwCqSfJUj8sUHOX1oryIu2PFrwAvdW8hnlkWTx2bHoqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBVWtVCAi%2BMod2P%2FWircAypZuy%2BQpUGLuqIgIxjaWxDvIVdbDvDjEK5p5jCuYfep9dhiItyjnDePsfHJqVNLAxbGw3OrYqvqT7nBYfnzqZ7atdlEik22GBXPlVQyKuGq4YozXJBCWldePienOwfQa5gdxpVmVcnGbwo81EpFqApjoQ2r%2FJb5fNqMrcksGArNOMdAvSSZBt9h0DKDBI%2BjSO8PKtNNox34u0It0qqqm4EXRDJfxz4t4TU7g7DozKpwASWRJGAxDIRx0ojR53IVSfZaDPNhm1k6j68j1JobuMN%2Fo8p89XlwQ%2FAqcxwzV%2FBM7UXaIIt2ZhIIQX%2Bizy8YoIXWl5yHi6V%2F7OuyglN4gvrbDVoxUv0OucDHt%2F8H8%2FaSihXfNF%2Fxczuuiekque%2Brh3xxC1ZHYDw6CHUBohC7u6Jedn2GBsoFYIB4o4nE3Ij%2F4S460%2F1WYobuIQOJetX%2FCL9qeVxlZdqDja8%2B9qc1DT4fdSFjSlZhp980LgUekjdKY4BVZxrsVYNGFtiKMODeLQh9OngzXr%2BTPIY3you%2Bdi0iLld0Snyqg51Db7WU9z8dIjvUQSXqeiItTHiDrQcpYwuWPNoRVtGhAg3DRQb8qWdlB8JHXxE%2F254mLhQWQhzRyvE8g4wo90Se1FOVMM3onckGOqUBnCEMZcYHHcbLNehl%2FHdvSMCyOiT8JtgzevSoyPCe69yTpz%2FBZunSftJiLcFahxnOsojI%2BqcltDmgE%2BmTm2Q9c1y75txqu9xSdicTc%2BrPHv11AA6XKa1IJpSA6LkKg2YN2bWNWKCkOedIVXeOp0jBLWDhbVg%2BpsVE%2BJjmuQ9joOLWUR1eEWexpiDYmY5DDVSnI1zK7hMbojlrllZPszJUcRlqoC32&X-Amz-Signature=a159f77084d0ab71d89c29f03da9cb50a18a6a972e86528c1855f26f4767418a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

