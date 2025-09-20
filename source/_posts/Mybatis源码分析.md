---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVIY3SXV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQCqyi34dchLgUs2t2tQKsdJs0k%2FR93YjnVhOQIWsnCmEAIgc7QzoS4G2vjzGRcJTVMmIG%2F2D9xFXLJwbuYOZ9kgqH4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDANEN5Up3q9D4SM%2FVircAy3OxsuM3agtrHYeomRQObk2aasqx%2B3Z8SUPKux0PxajscL%2FGkRfZbkCb6DYpp4s20aHOUYj6ZuIXPVsxxl4asXkAy1gCVVa%2BeXijt7nYFEjPVEeQU5El4CEjw%2B6oyiO%2B%2Fa0p7fRr0eGo7cyIL9kTKBmmg2sBYuCLb09PvZnmqskiL48VFUJ74qLBw8i5Vxzh0H6lcc%2BLhM%2B9WwMiEzL0y30MuqS8B0y1nd%2Brd7lG%2Bzfalh0GzE2ugVp3P%2BuzQbRTsg%2BRjYG%2FLBw1uwm0AzEGq0ilNfCGSEaWJnVyZlhv1Ajyxh1DASTep5wuzEiU7n2R%2BJTo8hRUkbtAN%2FSz5nbmgtvO1iOP89acxUmwHd5YcYDR8zdkty7NmmHGEtcLGTVocPGqRjPI%2FCUeJ21FFrwsJRIbvR70KkGyvg4zVdaBk9xlBC0BitNM6xdUm5eeXs%2FikQGHuiD%2FBL8yRd%2FbjqtDos7oimglqYkyPAQANCvWf1k4M%2FkubfLlcORJHCyK32aN2uFxia1PSEUCxCo%2Fd1aC0RPqH9RpbquELL6CMaoigRJhAfX4NjHCdfV2sC9WF%2BHUOFM775VwL6KA5F9gGQHXjQaFtLPU3YtCRzIlK7E%2FKh3j87GtPVRAentsvGTML7nuMYGOqUBANtsNoGxFFcaOX9sTu3WGt%2BA%2F7BXxtkwgbu24Um19NkMZWo1SQ6zNJfhHi%2F3LBBeEBPRfzdJfdP0W3wkbF9R%2FBZRgM8zS21LwedzgEJ4w5VIi92%2FusegfsSpw82v9FGvJmTMzKWMImjJIPn3S%2BM4Wnei1UycYXzPcGeafBEbkH2S0%2BugrEFuF22wABqm3ugyO2AB3C0Y1CyvoMjQFWv1eN8LYsau&X-Amz-Signature=ac88255ad9abb855d917069b8644e572ed30ec2593aded66030d240c2fe07f99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

