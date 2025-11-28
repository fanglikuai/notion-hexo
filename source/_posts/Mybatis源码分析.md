---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R4Y7JB67%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T140041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHh38yCvbAv6%2B9TRVj7YMrxIJMvWfHHGQXhtLZ5El8VwIhAO8uLTb1g35Ei34r1flZTqA6CJ7pbBzN50N64c821a7%2BKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGrTokqQMSl%2FkL3lsq3AOfwbMFMc3XQt2uX9CfA4Q8TY1lTmW1odeY8orjLO%2FCyO4LUMrkGvlJM%2Bhfyi5vpnRO2Hpj2zvz8E9oliOPKG0GlQxRSk4SiYwuZJDVTgECgqHH88KI48Va4JJ869REuF8xXXzEDhQvdhgqqW9oqxGjO0WVEL5xM5BlSm7OYUP7J9vSV4nG7ucJPFQSmDCBowBCi5jRd1v1LFx4kTjlpMejGoTNPg34vYZcPcRiqapF2%2FkMnvLEwN0%2FFSB0PzaUi6XT4Uudh%2FwOMrVxS32WM5avfisg%2B4L4r31gIvXTiEsHH0g9P9W1%2FKHjoaUHngMuAgqZhHdMUqQiaXbKBW9xglaTfvB2ErReHS9vXP%2BzKeuI63XPUw7GKDuIpAx9OWNuKoEmrhqOi77tzjgOt58JYhvt8b38eCFzHgmvPmJE3p5RcWfRF65H5DxlUzvipfRvj2%2FbGlL4k6Js0amJPtJPs0iDURWoeCXTWEFD4ivEtvPOjJXD3TLK6UOS87YiVmuaNEloD5EQi04VRo55wjzMfDVecs2NLsee%2BCUoZY4kYQWyea0yEnm8J8aoJ75TG6hebQvu7URy8SCLl51bnaxV7zEMKLxv%2Be94eR2PlMcA8GO5HrXIH7qKO793NejSBDCPwKbJBjqkAQ3kUd6w1piW%2FGCJdUHxozj6m8hABDQz0NJ%2FU1EkI25KEvl56ogWK6mdi1Yo4avsN53gR4Owxl4g2zkcZnc0ZdcAK5dlx8yyxnsJLbVIVbx0dEi%2BVj5Sqj8zjqPMw8zjSrzQWPtAoa7hQB%2Fx6szSJU9cRVWjt3XFWGXtxBy%2FToxvTFfGlYm1ry22kH73GmcPHgG186hTu%2FTmWGykdGB2jaNQFapn&X-Amz-Signature=0da63345fbf96789770d817cee970c146e439802b5855b9d9c445ce1c6891c85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

