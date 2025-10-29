---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WTXK43Q%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T000044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIHjqFkSOQMzgX%2Fc9qncM8umQ0yeDQHhulgR1pKsqRdM4AiAXV3isHYLD8Fj79ihth6w8asy8eVT5UgcMBH2uyp1idyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BDrK652v13jYfz4zKtwDK%2FVmB3fTId3iJHSC9gZNoMaf0DZlP5qM2vgB%2FJt0QlT80B9VqpCp3tCIAy%2FFsc0WHNXkLht60WkVcuYITnL4kWarzL5pSU0pvnr8%2B4kqWWprwk17hGz8RME5wIXwVcXZ2h%2FlvNhC%2FmZP0KS1uxYS3Y2JuTCwy6oVTVkQps6BRtkkreKaBopIRgUZT5M1d7TGR5WyQGWRvkb2pe96%2BCdhqAyNdI7oDzRJ1WVLEkaOzqzdbRbs7SokSqQtZ7qa0lV%2Bw9GodHv3dtQPtgh2bcwc7q%2F%2Bf0uqy5VTkfsMP6TC24XI2Gjoij54Hep1HpJHpeQI5O668gtd7UjzE86oGl0rFUxpFr7uomPKl%2Bp%2FGZmHj9D2eOmX%2BF8Yr045UOk4fC4xhUN73LNcbSN4IY0va1WSdGJ0Hj7Jzu1Fax%2F05m7ZaW2FbfW406E1ZBTE%2BE9TQ3xlLBmB8NABRjRq%2FX4edxsL7dL9SfwaiR3TvkK6vBaT1wZL%2FutsCW%2Fmdh6FMgD0zqhnyfMcV4HYxA3i2hH%2FSETvxDiN7pDPIqBQJh3j804HQqza1t9co0WJARGyrt6W4SiYuuidAYcCwXCZ6vTpKOoQ7MpPoCn1tu9oaB%2FDvSWILcKVxKMKnL0nNGmfJwcw0ZOFyAY6pgHxg%2BV8VZ%2BfgWIkcMsbx24Fjx7ztPvjWIDwlL6F8sypKH89jNgpUsOyOWms%2FMwDWe4jZl9yNcBokFXC4YLLGXFPyZZh1S9Hdj%2FfkSy82jZrT8fYfibuQWci1vlP0fOz8maVSk4XN1DK%2F%2FaCsKiWYkTTDssuXl%2FYts7rJwTrmQEL0Ltg%2FSVvU5qY%2FOupbyzGF%2FCoJWCb2bFY08HZZD5ji89JHsWERzvt&X-Amz-Signature=9264b5be43493ec5ef2e9706db541a4a29d5e14bd281944813c0d5148ad58230&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

