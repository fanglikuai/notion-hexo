---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TETYLR3J%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T080044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIQDX7OiQUm%2FK1%2FOFH2PN3QSAd8QKtEL3lbVpGq2PewvPvAIgXFFlWDMt3ITS4cIXi9j6idhaTIfc4CIYYAlAZAldBDAq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHZZx9FwKXFUaGbgYircAzJRp9d1mCgxDp5vKF0QZd9uCCkwMv13vqhW4Uqz04AZgzYOtlf0BC1rKWU8Jb7%2FhDFKAJGUeKbRzuiE03hPHE2tF2HrYJq%2BgOIp6cUEeNffSrqjmB52vhE3pdrUUVS9ft9ILRmsLvIRTgyEaxXWAg%2F0YgiUrCazDw95FVqqF7UHCpHgS5P1cRynUupFUkoAiBTX1V%2B2GdzvPIJ4zMLxYQJkOncGxk42KWJLyytQ3%2BTdTNR6VY0NZQ8kDx8IJi2zhC8XARchUWzVFYY4v2PqSNFRmCh%2F10I2rw%2FsvPRlWvy4%2B7y0Iu8t1K4itngxFzNXrhviYhJ1QqiMDUpeC%2Bz7NQtrSgKdzmjsLAr8uj701AlVGomntfidX9JbFWC4OhBuc%2Ba8Wha%2BXu%2BWqfefRD52IyQIFNn%2BYy%2BdtZRYfLs94L%2FnFjbqBWalYA%2B6FGR29NBthxwuDLMyOajcByW%2FOz7G0FGKM9I7eB4qutnOIeJFUm7dfpKGVGk%2FNBUc%2FBzBCcDpUo1NbdLt2YOBEMAV6bri%2BpwpLEe6z1z1pMLM8me1xox%2F%2FcpGKIH35Z3GfTg4WbPxtxsEjg%2F%2F%2FoscJGcmauPtpKCcWc8Ua6JejU4XqAtcXmzcW7bGcZaf0P1wS%2F9sMJO088YGOqUB470twK7Q8DtEFJ%2FGcJrBwcs681gareqaQ4KN5CBgpMU4KqyqF%2BPWhXvECPTg1xWS9TsjNh5i87pkGZ65XdOtnbYLTsJyy4vahOJx6dKXvYrQ7I3f7rY4X1%2BVtOjkpzbisCS5XR%2ByJMld41%2F%2BRDdjBL4m2gguLVFpn3Y2oxRweZPBOpHd79AcDUaCySWIHDURXTPRpr4yCUMQ3pULNQm7dzEHzEva&X-Amz-Signature=18c94377d488948e81394bb3299d2ffdaf10a946ff9f64b0de8f09cb944fe85c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

