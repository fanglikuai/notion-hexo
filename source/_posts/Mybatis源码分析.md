---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHTLKU6Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHlcPGfNU8U59Ay1EEeZoSXKtbxH0w73dYMtgRoYCeavAiEAq%2BPvQgZRlNcGymfByb4sYIRNaUA%2FLzExdE3dPcJdFXgq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDBUd7AHrimbWabKEBCrcA33apEwHYO%2BPqcbkN5FbPoBvcVKktD5ZTKbk43AzJ%2BxtCVVlCNC4idJRzd3LxZQHXSv0CrLX%2FqvKHhSVjOjTRZcWj9bnnN63G87q8CGzQkznzaZrnJP4piGgA8SUp9VkxSij243YP5L7zN1z94k3fC90JQDZFe8C%2B%2FeCAyjrHUDETaksuXUrAghLIBwPDjd2ST%2F6%2BQzOjaEdOlE8mFs6IQrL5JrP9%2Fw8TLkQ3A%2Bd3x%2F%2FNHDfSuP1ue7T06sJ49hnFAD3UhUu0%2BKcbdzUXz%2Fc6ydL8tPKHK585Y0KYlgCKR%2BIqQ8PRw%2BkE%2B6v53okEISu5RvusjDpLswYhKdckW35Gg2NO7e0E8dwOkazngBuT2uLxitVGll3uhEJvzNcnFCZo7Tok5ruynJicDWz2JrTOj3ht%2B%2Bpb%2FOavoCCnLuUsuQF6lT5Scg3esyMquFzU%2FUSXdsk76O6S6IeQSEdQNf6IZbPzgyBWfezbgNzR690XAFDEVgUVhqMAwTtBg71xLV5fcSuL410qhhw52AONtJ0lFBdOIGdepRamJfb2lXmmsqLmCNNp0fxw2UY6L0aoPAV5b%2BXoeCR5XGXJJ51L949jmjQum8bJ3gLKv5SyAXDWx5NN%2BqFeZF4WLXSPz2DMJOI1sYGOqUBgd67G2fd%2FwE7SBHmN%2F%2FhP5GrIPeqFUGcK1IxU6vO7Icc4SgkUH3O1BtbLMbYqVHv%2BxF8dRYWyfMvpnGxTRMTz19WW9UFNlCb%2BIvt%2B0K2uHuthXUgUZutNGi6TspL%2FZkyuPY6O76Ok%2FY3sELmMfwiUDAG4IemRBE6CFKs9bXmYua9YiZikd8y5dG8KS4Ade2Z8Ic6qMj7cOtsL7%2B0dGGGF9rPY3mZ&X-Amz-Signature=5607fe0e6083304486c5177f5a5396a4da58b8ab54b73de8f1b0d30e8ce99808&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

