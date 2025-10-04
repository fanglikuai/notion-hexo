---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG235KQR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDL%2Ff0guQ40bP0GJ4TFA56TivmOCUNbARS74rFhlEDqIwIhALSXZND6Pi%2BW1WtIvJyH1YIgvyJ%2BLtbSmAKNLffLj899Kv8DCFsQABoMNjM3NDIzMTgzODA1IgybZM9iQJqc4%2F4Ujygq3AN12uneaze98miNpoNqFsRt3UFjeRY6htyGzTyeMEmqUnHNRGcf9f1LmzwiwfrtEqKM9Arp9MDifIDkKBrkYJAd%2F4DKRkXO8LXuOZbUNTm5sNHHVLpjtDbBNpty2dyNHO4zYSvpB2UUV9yLzGZDtZQviQRpB83eGqXj0Q9i747oCIZJQhCjeBW49EaM3b9yBpbb%2Fj2WQwNnPMPE5e%2FF7R78ZycL3v8x1lsCIabNf9Y4QWw9Nclj7yftMGEFnPTVSincidtbzPUxs0z0tboGEkFt9IFD6GYwBfR09oT74vlsODI2Ua%2F5M9Omz4IdEn16JGRh7F%2F5RoNC0%2BW555nOPrbrK2%2FsjV8ur9ZJOMUwPLODkZjD3imUGTEX8LbZrBvDuEZORRKjNwQiVsFUtV1S3OZdt7Z793B8%2FDivok9T47uqTBLe0ng%2B1aNUvZGtf%2FUBGpS%2F41i9dlcfOizh8gA2hlyOskVWJHcnvbOQLrSUufLdi0CC6XCd4JPEnkJ1JiEv3%2BL%2F%2BXzMpwRc2JOxEx243j3%2FvlmGB1UjDv5LNlYUNdF4QpiNbq5opWpCaP%2BqArIIzL4hvWohQVQaoCxmt3XLyvu88q3rFygdZDkOAo9QKzjRyzVpLktJowYu8Jd%2FSjC%2B4IPHBjqkAZ87jFImGZK63PvP2RGP8M73KsU52EFUsoNQFeaLtzaZoxx4L9%2Fsc1%2Bn1je94LH89xpvWSarTEw%2Fff9J06LOImEUskb8WkJGLs0hx8hKUr733%2FIpKKHOIBYNRiG%2FDxsK119qtFO0Xom7LLW4YLS%2BpR6%2Fu9ackkiF2xJeFIbS967KnNJEIyCqH2fuFH4YDjQLzLpLJSue6qg4KJ3GFjI0dUI9nhf%2F&X-Amz-Signature=1036cc5d6bd2600f24b7b1f2d718c3b96b8d2a4da7a67fa6616ff0c88b278542&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

