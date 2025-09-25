---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QOPTCRL%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T180114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCxDspVErKYxQXzt8cEw6AQc%2BkUbWjY2XDWlav2%2BO7n%2BgIhAN%2FxklsPet5nyyLYlFziNJys6XDkuQXble4rT0XGnF3TKv8DCHsQABoMNjM3NDIzMTgzODA1IgzJVBGL19cp0Q3hu58q3AOo8UvZCPZFniXwMTHzU7Vio3pUHFQgzmG%2BLIG1FhQq2XWxj6jhQGawSksVw07feS%2FiQYnlkpaTctAPejj6zB%2FKDsdbQoTSdGHLcFpgVdFOgaZPZ5xlrMD5551oiL7yNPvSfdOYAjG%2BHF2AHJAZSGW8B5x9vG2Z6IN%2BNZNtqMCL9f8Y3vDDVr0G6RMttZEz%2FfN0dMS%2BxblTBHPf%2B0HtOmciMlztkxObmtgq2y%2BDI9Ky4y%2FdUcwS83U5SrSlI5WjkcUQ3SS2Roj72DHOurVb%2BjGBw1YgpRDNFCUnqFSvfx75ibN98yZaO7Hn15VWSgDRaZTrQhwg8zyDvm4dO%2BrRS0hUM7MI%2BN7N6GKo%2Fv%2Bajdz2mU6G5YDRAY64EzSuZyAYg2mDqhJK41WdTBzOtYhx03ZoYkkSk50plAun6HeyhC6W2EUAgkllDlDR4twjFguD2w%2FbSIj6Wn2MbMIjKgh7FTY2f%2FFGsPM0aFQKmXV0yyZDj3tNMs0RQ9%2FFkWeR%2Fhpf9Yfl3COxYtPkAzN%2FPuNEMkMBhj7wiq2oElHc3KMMlSzRSyjB7hni%2FM1b%2F2qXqCE55%2B8TZh3dDBPz%2BKjmQ9TOuq%2BRXfzxZ%2B7A8wYJug4Fn1kNuRl61ixYh1FTZJoFtzDi99XGBjqkAdDvZ2PPUi0E8IYpKyIt%2F8Eo1U9T4xx4lX8sNkmJOai4chgGqOn5rZthT2j4rr3g6kQDz5%2BSXjWJloSPPJ3yxNRAkLmbgVxsOssVXy767XVNE6rGu4kCAVlnKPr9NjEPxUVUwxMgGHoyEG4EYSwGB2G%2FqemKXdUun4g%2BIdQZzmxbSfySGFBgpxttXhLT7VN2IWE9FDvpz1teTnFePlq71t%2BKi%2Fub&X-Amz-Signature=005b0335c556295b4bd83d5ebc3b4a108800a21e83ad842a7a5b1121253d4863&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

