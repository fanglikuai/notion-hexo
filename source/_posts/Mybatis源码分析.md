---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZ4XZDQB%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIBmjsgzq0TILy6yvDRRqZW4Th7FBpEQhW%2Bv%2BUWW3MfW6AiB8pV48kvKT6hW5OYFVYMwqaklW8PxXQ9licKeFOxMYlCqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBwJsHiIuFQ6EM8EcKtwDIHAScLL6kJzkT8fmKZtmOEPGS%2B7abNAk%2B1IkJxC16qCFudfvIbzMvgQrPyFDRN8WrkXr9bxFe2W9rElE8gsoEeInHaM1oO0cvpRQOemKtf9wzigc0RMiBqAiF6SvxB25Nuuvc3t%2B3DUIwmklt3BZGe48GaEV11R9%2Fg4P7dDEJQp%2Brt0NGwZDPpp8ihNNfhIW5tsbJ6jZQwfG9%2Bj88%2FO4BuCsH6lqVyq3UbE2Ebh%2B4A9P0Jl4iK%2BUIo2tkQm%2BF1H3wLxxTewcg1koUBYG8t%2F8W1mcJqfCu3L5RN2HvOciBpXOkTNBR%2BWpp%2FPW5vlonw5257ohLC68ldkrfmWIpMlCA%2FN23VxEDzye34t4A8VXV6Mxyc%2FQSnRXX9L4kWsbnRajHDZMy%2BuNpgwM4kFhUW9thvKWfmAQ7Sq6619tAaEO9P%2FkrYH6IpqfuMgO7Q5qGywdGWNOd6E1EKioLngBKFZefHNQ%2BiI%2BmMaa%2BdLjiPx4yvTrteNWw1F7MbIjzVeYNUjg3E%2BY14V1Nw38y6bjLYG3Rl%2B7eFZTp8Ke5mvNRNjTI15P7WDMPyd7271LwkjAuNmKLnjAtK%2Fis6WlxD8ZE1ebE17Te%2BzBvY2LZM%2FvHhNTGqiu%2Fcv0Ur7qk2TQV00w6%2BipxgY6pgFLO0JfSRoxd6HxaoAusMIy9aVo2%2BiBxjLohuxQwSLJPOxuyinQTioZZ4HNIbI7OhZtZ%2BUoZ%2FgPQ83asAL85EN2mrkABwmkocLNGTbyt%2FDUEQonE35HPwpHUeO5NGm95NEA5eOPYAR5foweneLUWJBqqzDWGlHQnQkA7Tc84LyIv%2BPnrxevTnYcMxCJaxSJorqI9VZ49U%2FBNJ4HdF1CFzDj11%2FWqL1q&X-Amz-Signature=dc7fde2fcafa010c733084f537d31a9b82d620996b9fadd30199900f51fe58c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

