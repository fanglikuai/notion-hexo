---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ2M3ZLM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDA9Wz8TXS781U8Jiw6IF1XjbmG1CEhk1MPNGn0UGgMeAiEA6CEQGQf%2BnGIkoGbOR3Xxl00O0R2nT4dMi7N0WmP%2BG%2F4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNutSz7psuMh4bUy0CrcAzkqluRfOajj2koG1IPsgibcEcirRcKWVdovmFnRau1Hg9QOueJ6%2Fok61WOD2%2FxKEN%2FDyBeZMO9FF7tokJzCXCvOSoOhrPj%2Bp4yOYiauuQ%2BEHJg9nGNP9IVIGlGhdOBwN%2Br34kGjicC23vZ6YVKIqhIJYuS7giDNInSnAuaLGxzS8tVAhmD5FEaTsgwFI80%2B26BpIltKLFacisBSILjQV%2F5cxWT%2Bw5jVUb3%2FUkNjJQDvj3dSyw4tD1XUeTYWezqLbvUdcIvT4ROPGxDfAtpyIBJ1psyWi%2F93Eikl%2FlWVgiiSR6oEr2TXPTXVNuej%2BclpY5BpyilHM8QAn9U65xSu8r0aJm0cYJ3Bigv7KvF4BMusk1mSSJNM3%2FretonkIZ8zz2tQ1wa1ZJeV%2BMY95ARJ3igwJBQjEmpk9oj%2BS%2FtmgSPf8TqsvVobsdrFhYo%2BpsVWyBqgRD5HVN5nvgC2EyyaSiYs2FMPu5DzM4KYjPA08llx0Cj7dVfGmwiXIXdnpQEVRh39FB4jm24M7b4DrzA96dMzSgFDF3SbNchK18jp%2BQNDcSc5poXJ%2F5m5jRgXFlYtzpIDU1fcr2Ti8%2FpNVZ9fHrd%2BkZDBKE2D64l3sxl%2F4mc0m%2BkeXzdDY1MMfiWCMJeR%2BscGOqUBtgne%2FUjd%2F%2FB4PYVUdxzFEDmoB8k4AbPNgxsnXCX1xpPls3N36Mbq07MR1Zze1igsVYNkptF5honuyyIPBIArIk9ltR9oXJT1ty02Fvu8muq%2FsOgOSm4nwBjRB4tBjOK9bOQKoPvcm1Hmka036qPXcTj7Dy8z6VX3WqJQCaXECu7ZF5HwtdFG35mNTCpRuPX1tuVnF0116OLkiq2139q1yYhUHwhA&X-Amz-Signature=b77ebfdd65479467db9b15b0f29e0ae47d41c079688694fef9869bffad434ab8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

