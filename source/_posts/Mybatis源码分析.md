---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HHVP6G7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFrW1qE0%2FeqEHiMHL7bTBaBTAj%2FVpJrI5D5OBzJ2RgdBAiEA4L6qDzi3r%2F5hpbI9SvFrKMuRbHOfcfK5ZxvPhOH0AO0q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDBRPI9Xewkfu7VtehCrcA4dBZ5P1qgl9oIpdB8n30T4kDFK%2BXAapdCIsnQL%2Ba7Esnh1dtyQ5NL2xY%2B2fJXZJcgszTIDlcpZtQRITNcsYWnPOP2%2BYGqC7I838WPuAP8ja82lKCX3QcBHu0sdPPKJvv%2Fg8cDKHdlnyIMWrD6WtoSlYzu9%2B6gDqNJ5M5rAC43HJuY7aaOdb3nL2CXLkmran%2Fx%2Fe6CINldCYOFt5ns7X0hqKGzBq1w4hhiwrPuPtjk0jmHp65UrLpFBW7y9%2BqesfAewULvpjq%2BuKv%2F4JPifjL9kcri6UNlhp%2F%2FAef%2FUBPyT3Ug8x%2BH91VvSg6FhwPiSptPe7p%2BFUiU6d62POX6LWQG19UBsVJ9WlLsdEnf8HLRNXUAkccmdA9jh9C1PWpYHKiaZ2Mmy%2BGImWqOKq1Vi%2FrBUqghMa1zqqJ3ZLhkOpHQTAKW4%2FX7yEOvVLGllXIZ64K%2BXz31Mq8%2FTXfiOE5pH%2FDlF2tk2W3oXM2z%2FGdKfcY%2FEz0bLW3igFLkWYvXrMH7oaX1xhCMZ%2FXJFr1rhQFL6%2Bqa4auAf1tmmnVlSf0vYzDCo%2FEIVZxfhIBK%2BAbk4zuPFaDYG6fLICQo9jS%2FgVbMVI2BMPmn%2FrR%2Bbym493%2FlG879njTPil1N3h6BUWiryBMJas%2FcYGOqUB8PTaT2%2BfS3PoqF9lqPp2gs8bK2q2VeAa9jOqUbuZk%2BovEVQxUK%2BKHKYTzCee3aLPlF4Ca4Z84ZKPnWwO4gL2%2FIfxHmk5l6oXJUr0Oi6pthmwAGWea1ntNxLu2HaphRwpVUBAiMGJiOiflavsjFvT3PRYsNdRCaipwEctKeEsyoYQFQWwDz3wExT7Mv0KFLtgqdNcPfaObwvc0kUpKQ%2Be%2FrpMPcLB&X-Amz-Signature=c068cdf3afb32099ff4a033bc5e54ba4d6af683f977b3c8c831f1d7876bc2c1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

