---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466727NCKGJ%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T160043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIAFvFnyFpIwXUcoQzzSPs2vZCrT6y3VEkw%2BXW58PVDB9AiEAvpIlh17qaNtgJsJWGuI%2B%2FVUcDl%2FrEi2PaSmCIdqvGrQqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHkC8sMtFnJtZskChSrcA08OuxuNTt9m9SnB9he7ihajJFP6OHxfMB0bnE4%2F074dIgQmeePDqsjbzNsjgAWmXyEZk895F3Bllnf%2BKrv4DPi%2FAdchqy9RPqv3ng5D5jR3JTeqbY4qmh25sS4FU67fCfQ%2BEoKsG%2FaxMfhycy3i97vo7ID%2F2NCFLgcfzALXu5JAARTpE0QAy8XSxaE5xedBK1wE3wlNxIqlrIQsyH9OtCNQVGF4nKJP8adR0SYQZzYeoWzHeZ1uIGt%2B8OiMTffcpDTz1BUDxwlsoqQ264jx5zMWmLAlzCOsz6POR4vO8DhT6ZO5y%2FyarPkjO9wkawywCqjBbl7T2Ka%2BXihjAgh9kFsoyGZa5WeggENoQGFZ2y2uzpqVnHn3lYyqWYTqXpDtjOxd8hrj6j8rR2lP4GgDpMQ%2FsYLeCXd4bEEwz00ZSmzPKMhjx8E4PlxOBK%2FqaD4BKP2bVUKggrCK8ILiaLPwgDqxfRs%2FFu6l%2FnZS%2B81SJgmPHlP2atl5Sv5nhoSc9YE5GD1YZiSuR57wMhzIYcjgOzkTXIBd%2B3%2Fi0kgbxFPtXKfEYmgJmTMXBg1EKLBiqIJRtqXEupxJ%2BT6we9eCQcBX3%2B57uF8LDPtInIcH8XXW2jSMC%2B%2BvVujO5YWs6k68MPXppcYGOqUBHs6%2BHsjf8XT84OqnxuwI3Bx2Z0t7NyPet77pYzg%2FkwyObx14OB%2FqA8XHp1i2X3dZVWtH19T51RM9iCwzjOAaUDgHfTcw0IU25f6mhv%2FCuXQv55fopdsRQNcHOK8ivius5aY%2FLR7aHeX54pwpRoog9H%2BMb%2FhSHQcozkrE4ibBintr1zVDc8z2TYhCnQGUZhReVDx5GTumWg3HD4s%2B%2Bzx1TSALBpBM&X-Amz-Signature=3a98ad163c9dc03679dfa87ed4924be8684710b5e49c1fc169fce5af60c8ab7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

