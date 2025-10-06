---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663L6PIL4I%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCC1haMb8PSYZy7zVmRu3PdZlNmuofpwAYk2vI7C1zLNgIhALu6jOu80jMCIFhHDLBETJsaur4nePtIMefJFJFOZATeKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDyCHanGKcPeQ2g60q3AOgatAj%2FEQSOVDxSdEwE3heet7%2FBQ82P%2FW1PzCzBrjDERTzWtY87CKci0Lx7LHQPKurLf7Y6lIITVlS%2FUpNUhjGbAwYhIe%2FUvr83oO7N3d8is%2FxPA2e%2FWxuIJY7GXYo56YXLs8Gj%2BqXNbYD6x0EKWiu%2BKyilRtu%2F5yx7aIvl%2FuGf04k8WTHJsn4lmXqPGg%2FWj5%2F%2FtTnJjjbK1E1Wc5McFjfuuzqiG09XJ6DNKe1B08cMzIiiKt1ChaduKCwWdQfT8ElnNVWJJWVbzGGym7Q2uFkm4rM%2BDL13weuRqE5DT6qacry23rGlBdKd2S0Z9QxesJ5qjm7qxgEqTwhA8xNWdORYb0nZpJ1FvH5ilFEn7HAdrv4agRq19AfAFFbLlmleay76Juuk8FxK%2BHX1vV2b4dQsPT9l4hOWFbpBt1%2FYOsjZIzgcvQBby36ae0oZw%2BN2f9L7l91GCWAJaDJLpOqBcgUDvgeFu2KQHds%2B51%2Fy7WPG1xhr4ojNrZWrjckaUEGxyX6ANYAg%2FRsoWPUcrnP1gwfYf4YA%2F95FqYZvWj0EYJ46xA6xlwvJFifDJdYJ2PHppRcb37bQKDxMHz%2BRCRVXEGFTFAHJnhNJg0PG6PFbLXwlUH1VOtxvcbfZwu6ajDgsI7HBjqkAaIksmO27wEPUQa8fgXL1gyM91hKJ8uKRddobYZsGZYhmRJktdLw7GMMch6fsgMaitHDJgiElQaRJ6P%2B6cuxzRXgGE%2Fyb15hTictBfJWyaqiBX%2BARUDiDSkSxcofIy%2BL3wp0iAsfCoNhRjeKuRsChDpEzWhd7VnZ8%2BRx%2BQRzOoGm2MYt2RZ0c98bzYbs2AJaawTirsj2a4lu95uGAtAGhcAcUkY3&X-Amz-Signature=442f7aa7b2380e47a3dece0af79ecf971eb83ff1fbc345fb0dc2ffc91c236023&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

