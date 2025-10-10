---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUIHPF57%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIQDSKlBMpJ37wWinKhNsl22d2VPyRL4Od89kFiwLhaq%2FngIga%2FHMbqXC9C5YLlOBNvdkiFFCGPwMz%2BwEw2tFD2iFHHcqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO63TmJx3ARNRL4C4SrcA1ZdBJOaZ8BN1OYb%2BBtPI1zkEr%2FtCVIOGzGU%2B7DdlMu2CZ0%2BEobJLAGOpw6WB7lFDS3WnORxwY3EngIOen3GhZ2%2FenRelIyEOHoUn%2BuyUh88yuOXKB%2BcwSFBhSOfYxwQqVVT%2BEOAM9S3%2FadHrQNGAxyVZObtjjMLpOkDJvNpEp0l54IIX7jZYfSX0TTeKz7DqGSt0xeiHMeQahn2LQQt601dC7qE%2FfIeEamhB0q7cGPGsScLPRcWwZ%2BS%2ByHJQhT0sItlY4gsp%2B5aJgrB9x6j3IIKA3KTjXHLhwkafkSVW6ZX9YhAJu6nQ1cF9IUPKfJ5Lu62W7HTDlh7fC7B9fxI9u6X6YKBj8t6pObUVhfjEMuE0BwQoj7xeKi0yIDsmz9boZu7mduPFsCbgedW%2FXMj4SYDmJZVSZKeuA3e0zmYCzUVpc7zBdz8yySvSxMozV1oOP3oMhDVU2cJ5%2FiFAOipV%2FbyNKReS7G4LO7usBOd4zNIeTRL7ag%2BesJPxJDnema7pyBwAR5AmPwx4gnWBhQPi1b3Rl98DsbhiVtCodeHnFVMSRShQpUmXFYn8OdZWcwl%2FrLuaIP2KeBdanTuhgUTHrPY99Rb1YhfIg39Cb3GvVtIEZeUEhhGMoH2KvMzMKDmo8cGOqUBAFqeuh5XMWZs52w62e2TMDJmkcOnPh%2FGEAnagDmisGhbV8xHZvWlc8qAI8Nuxy5ElLvL7s5FQK2b%2B%2FtDLzJCjbJIi6Fmb061QG0jm53MRyjxA2fyX%2Bh9y8twSVf84QlVOSKPaPegsaPmiEefwbg7j2f7DzuIkgU8F1zyx7Y33aV0v0Hg0gWj8ljYZJIq1f0LRMYLosM7KOaX7nDe8wDNsq1cTDJk&X-Amz-Signature=25730c68f2b708ab0597ca514f1b87ffc2ba68f34e3276ae3dd38e2a951a7e37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

