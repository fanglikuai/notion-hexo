---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666MQ5DZ2H%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDcPAq0LOYPOF%2Frr9pZ7u5oIxGeMFe%2BWYqQHCC1e6YP7gIhAMg8XODWRlMFfbPQqanOO3NXbAmUModGi%2Fzt1QS1%2BkEkKogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzAb73j6ijMhu45B0kq3AN0cIlzftYrSKbUFlAZVvEKLgeBn2dn8DQCatlMhKHZQk1W67hcN5x%2Bh6Kf8K6sOaev0NtQ0WqqdLgUbKwtQDl97N2e4RWtBzYqaR0%2BBMcfU%2BtONspObmyOF8%2FmFnHyayEDQOswsGs%2BTybszXaKCSItel4q%2BOAdn7FjOw9BZOOnp7oRA7CgQJo27wC8ZXfgVxtln1k4ChlX5bXrxj%2BZJsZ1hm3jTkxFLgveqd8D%2FopC5F3DG%2BPlnOHM51PHUXibCs5rguaEFgcyQLb1WwUYwmqTby1VL0s7VKQX7ZeAWCZhi8TzVIzoTPDI9IRGZVi19L7fEy30DK9nOG%2Fl5t1ijQWw4bs388x8VYwLBtM8dUT73VWQidPNy3caMiqzNNyXJxB4MyFYBSVE4kXjMCPQQiy52Rqab%2FQKwutKXooWfEdVIKAhTLu8AVyAO%2BfccLsiPcIc59dHivuyNbOVkWTorvKb%2F4zu%2FEGOMOt1%2BCTYx1MHKr9%2ByRiP17nTH1wbvpHpGtOtt8ETusjDGJ9nNBfbEuNcnESALhzJLeFQ44Ee8N2Cm3ner9O4kpaZW5IYfqHz13R8JYjgEhQybvIDuXlntV6ew74mrAuzYHXo7rnFFN6u9mAgvsiqLrb4CYLCmTCpspLHBjqkARwfpPT7X2lWosEhm6E2TvcnOo9hhS8dxoyFxrDfpQ4NfGPJ2qyhoz9Ok%2BRZ%2F6I2Zh%2B6Jut77MyVdqaQEbAko89DkQ0A41HHcLvJuMU1HHORe%2Fa2g8AVw6ALEwWhsKuyoSGwPowowzVM0wcPfUEWfZ77vySnDcpHXRYZfeMbAMA2uiVauA7xdDhtz59fJVPcUpufd1ze4JK9u2ED799wREGxkLG3&X-Amz-Signature=e8fce8ad23fed630b3b81da692440deae5ebb7e38b30f98b5f0b5414df35df75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

