---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSZ4NVRM%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7MKLQowdzFWKvDEt5v9KrIt3IrbNkzctTRsZ74MWOgAiEA8SMJf4VAbTgbVNIpBOOTjayNLCq%2F4UIv6pJnGgcIsp4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPil8G0nqj7lcqaNbircA4AkEiEfrIJf8IcXogxpda2VxTR74ZA08EJeXNijUtP3zTF3t%2Fu6tlTTUF9Lcf4A7Peg0zkwcnEfXIblHmXYw%2B40eD5Gj6u38lUDnxezyM8bEx0%2Bj7qwY7xRTRuqtvdWrqR%2BK1UA%2FxdgIZeObrCXud1oyYNTuWJCtpkkzGGVCmWg6%2FZcrt78JaTVcQ%2F0FKHmiLkxds0lwfdWBDElA7f2HfYaFdmQUV6O7wPAz2wEQcYGddic99EmEwJLPU5gIjOR%2FSHMiT5LMJLw40aLHWiatOFiIa%2BDg60EkoXK0PHjRsN8InwY8drABqjPWzO7rAu7QFxU5%2FALotlYQ7tXO2OMtN7J84FQ75w2MpW7rdbsMxQqlH4xT2B%2FezU9acrBII%2FsiB9nn3dp6SGp2n3Cyf0q0GZ%2BKr7Smi5B%2F8sV0acgBPYeKn29Ws%2FSNKp5u2HQsal8IfGr5ay1fETxBK5COduD2mk6p7n10ru9KYM2JgnVAh8cf7LXUDpIfZ2Bu5qvNJ2Wy7YH%2FdtaRfQ5o2nk6ah0n0lMDvEz48zP1Go%2FbOciViek6%2FbccmPDiBiD7onS4BDpcqGUOhK3k2pgYDRUsx%2BAbe%2FMHPKIQNobiiFYUSvhVX%2B44PTX6fiKGukR8PxkMNihockGOqUB3iWlpKSxMz2ny5NuuHzpL0V4iDjJvBjjM08NW7MAqnUDdTsAslzD%2FB8OyU%2B1u0DY490KvTaPHeT5rxXdiOCc907PD6Ujr%2FoKHZPhfWJTYH2%2Fbe7xkTRg0Rg4mBobxXR7nJB4gtX5%2F0wWp2wXqh0mjd8et%2BUt7%2FFmsrL17RtDcJOXzYqtD62bRrrSEkR1FWnpdEy8rop%2B2EyQx9QVyXA%2FDSlO70vb&X-Amz-Signature=f72bb21393e97850b6ad1449eb51c3707a24fc7b52b90452e9944cafb77c3d5f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

