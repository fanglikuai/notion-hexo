---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FLIQAIK%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDY0krBwkeajlAMDvWeCHfk7o6%2BTLZzk8Ezft1hiZuV9QIgYxqTrSHSALIbmxxcykdn5D8yrckc3adzw2MaBps423YqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKepHBvCHmp4k88u1SrcA1MQDLhTjdaEPpb6yH%2Bj0r%2FL8f7nRyL%2B0mWG6hvV8TtUJ9ASgLFsKTsxZ37rGPgqVzqrpUBbTGGyKQNmipuAU5EahfKha%2FQfH8kFiITuGtLWD23yo40WSB6ojjrMfX9dDOGPmBZodTztt4U0a0ltqmHK3BLJJrIQulzOEJKjiKbP%2BRAfLstI%2F%2F1AEuR636%2B7EYKfkEtLqo2NHDi%2BCnr6q0P9ZMYkr%2F1MTdX%2B0d5qqazCR7YAtEP%2Fe8IPJDqEFHnTesAvO8rghwe5hW998tkEaqwtMmiE4tr3N2XHmaFXjzZFKVlfdI916Ir3IFmZlFfylA5ZLhdnBmAL9nC%2FtZ2nWHtYrdMyEOfMDBUPACgejuhOQLGXdnJ61HWor4br%2Bh%2FQcV%2BBVjakgbaAjo8LJOxxXAmFH0nFSfuyaxj2DRCqALh8%2B7Exm%2F3TAC6SolojtuWTrBp5aHoidedVXU9cRfQdSF78OYe0Rk0fs8Zk3wLb606B8EfGAdWFArZcEm%2FOcBlXCIRdEzOjHUwZ1VcidvCOn3xI5J7xmqu%2F5dUnHL3%2FfgMhcjfe3eGPCdNYZO93si8BAQ9ZIPzTTz0A90pUaRdq%2FMnTJQlGR%2FMcqwbeMm5HTjlpCl68dRFd3wXYSVrOMOCTwcgGOqUB70Z8HG29IjnhKHv26q2CQyo7Q%2Bl5amfFq1RHww%2Foov%2FZe3eXvhRwL1YGhABy8%2FO6dKt74tTfTiAHc3H94ACWi1mdeYWTguIkawHylCD%2Bz91mKcuaikSdL1A4Jh39pGwIOETZCdguV3SzO2RLBecjmqJxQO0tlT92HRO0eueyXZGcStsAMBjKG601tkF5J%2BA7Lv5EIV6IS1TZ42yNXh7FIJrkYgGR&X-Amz-Signature=f6f52824b90a2d6f7c401d07607a66035ab9b60c785a7eb46f3d507552c07a8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

