---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QCJQBWM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSoEbi3tBGg%2BdckLTt5Wc7zFJIq3eBe9pCfgCK9L4gSAIgHA0F0yc8r%2F%2FSG%2B5eBD6Ubvy%2BZRLcEaTe5VbttKRYXyAqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQlli%2FII11nKUptJSrcA3kr9mGMqDR5lkfFfUV6EcrL0Q5ticHB5%2FbwEyrQJIqk5KvALapevq6nnh3hYrMCSE1cUlQuUc1Qe6zuW8E5N5yGTYp9b2bCJShM2sSpGo%2FnyjvpRIzZN9LGH%2F0jZgebFlI2BFlmqZl1tbiCPW%2Fb63F4V%2BxdsG75bfAamwuzboa1wh55ynFpJ%2BaXiLnzZPL1r0r0tNe7gd2c0oqRuEC1u9QKnYPhS195KNlm4OQDzKFVWSpxSksKD%2F9%2FVZzXUVphyxDZQnsMa6hkdAKHaHXYMc7Mcz6dRJSjuYMuzAZ4A9WGY76zrVU%2BbnkDM%2FVaW5h0Zry%2FsPciQuWYKUDL6nvIKIvT3vabec%2FhkLhvuxjFfQcqQbwk%2Fbk3fY%2BCs0BXHjOcrDABVyhKItk3GgNEkd4ZyNY4U%2BzaLMK%2BfjUR73iCDEb%2BnoVRjck34OmzJZ2vgUHVu5t2aYNZPkQeD512MTKcZTkwsmB5zerY05oV8RVWWQzcCLxLJlqv27581CpecNXIx7HG6xoCpvS84YTnheaGMfYYnftn4nz0FquDg4X44NuKW8KZZ3WhJmtg7rt%2FcgiFU%2F8rhFGk6Vrj3SU8wpz1LEor%2F2GuUIDMyOq72VDjrh51Q2akHI66QQpJCjEfMMn1kMcGOqUBD8O2mNYY2vMfX0baTduRzO3PWBxcfuNXKgyVXiIsOjt89RiRldVxBuLJ2wUspl8dwKWh3p21Gy4oZKKrysNOTjnRpeTt5ENMor%2BqPyWaCWlEys5N4mBsxGZ7kSV4M%2FpI7zdE1vHPZPmwDdjuBwwYhz4Iq2wvhF6mftlO%2Br4HZ8igkJplrcFmUAV%2BCSFPjMGNDpEVqEVyjzb84T%2Fmmo%2FmkocRh3Q2&X-Amz-Signature=9f8abdc2fb7e145e4a16f7258fd711c48c2af8698675b78104a85e2c8e4cf4c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

