---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYDQEPZJ%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfrDUH6B2vQZ6hay7feNn5b2x%2Byt3TwSdJ%2BQdiQ4OnmAiAmeZNelP2HaoNXMFCHKyuT3mC%2Beoqt3EzGUxjaafMCqCr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMJP2yLvyd9R3wSwKxKtwDT85%2Bv6ESD9tg%2BM5fFI6QFOvJodY8%2B2Fhn1SLCC%2Ba4Kfak8MwNiT8kHFwIqxyvoWdJUZOEayhP6E2j49bf2lywzi6XATwFbH6Us9sbuYuCdJ3inuTfcWmO1mW7WqG6vqbOUwfINmJ8dde4%2BG3IBkVDq%2ByH6Ul5g%2Bag420pM5GeYYFVTVFpY9RBS%2FeaI4FgDJDGL1hDHXLwozvK3hOJLSyjH0%2FIlRctn3R3cdmUb9RxWgQKi71NoCvBs8fk4L2hqXgFXoGKsmcIAle4DHmNnNYco5lHi9Gv7moiRgyw%2FIZ2nhXdOX6DasOoW2b4hNyP9opTFh5QT%2FGxQTMc43Lqle7IdVZpem4UfECQDk575rJPPQEVK9gP2sljGi%2F2y20JVEADPPl2PfSRpjjbxuFBYeQ1nS2cMH7LeaTnkyQiAvmEUDL%2B8MdghQbzaHJkXyarW0aA02jJ81HOpl1nOw3igg8DhNvFbPwu%2F4Yt5PaLrFYDa3NJPSfavOYCLh1DJrlmQgv7FXaAt%2F9e0nnHZOVunFqbJ%2BaOnDx3hFYdEz8UrWUWRDP3Au0J40CZYKbQ8KqlVpTF%2FQkg%2Bvh0dhOOEMRQsq%2Bp%2BPwm6F9NzyKPAQrBGZGVA1pcgZP%2FAMmdv47CDswj9POxgY6pgHdelQ%2BJh8VVFo%2F46BXMhEooL5pXvoNJWMt59hzNsVAJUOsozYMdrBcWrIFczrZwtUVrPX8k5iPYR6%2Fjl2VovmHZ%2F9AmF0M%2FmgJoaBfbEvBvdQuq45cRLFIxEfC%2FYZbfrFh18GnvbQDOcD2qaoq7SD074poajvfsfTkUmp0%2BNhdKr6hXt81jS1nbqCAw8HmJPJkgLY2fn2HEoVDpx7gQJqK%2BYXBrxsd&X-Amz-Signature=303562e14546b902fd54419384af7c390b97fd98e4342dbc3b588001740e52d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

