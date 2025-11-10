---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6KTGNK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDiLY66IWlisOWeT4QbbJHingk3eoJr1EZBfbxLzip9wQIgLZ0UnHo%2BDvdi3EChGS86Oj%2FnhvTwUTW5hUJeCBHThpYqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDut71d9KhxvhTKsRCrcA9Ty5ZHJ5AyOIpk3k8tjezu3OZCaY1KQqc9NgwJCeJmlGZm9Q7qFc%2Fje8zzY%2FNqvzSBvxdNjAmrLQYmQjHa7SxXZUqrX3oVzEO9JMF%2FqFBxqGRrybhGBALSukexQxq%2FCTZGiBhqLFoHaFxIiWXLJHjZdBORF7vd%2FHmg68341C2ol%2FGHttsaH7X9nbUZHGl5CBEhZ%2Bi1bxtA2sNwoKPDzbIoJzlNdDLzDZMxPuH6NzHjxmT1EfMs4bzrC5DZAnW1v8jVsahWjjcyflArBWJHLVWch89TuU%2BGXH4vN6UDp0WWKWeSQJhIA%2BL8zxS4Pm4yUGslukhlH1JCOaoeWLpIGopWN1zilU%2FU67K6xEveLRR0GHZ6U8rD4pIPw2lqnkBExJvaImBQtdMCMkhCWj01g4noKroWZOE7MHRrmJpE9xUGoD2acWgIzv3%2BifTtg1yfSEEWvx%2Fsqe7EcFCUusPJuD5xW5WB7RMcxZxbk5et%2F59uwNYNPUYF70Q4TpBuCJIOnGIBdO0MfjyE9Gb9QHxNHBFcPHri6ODLBJ%2BKVxN2mwl07zUWhp12NmV8ef41%2FM4MYb0%2FZdNhcchynsUmY%2Ff8b9dS0fSxi%2FvssNjEsuFWxVzH1ISovBKOgxbIbQGdFMPX2xcgGOqUB88TT5CgcIu4sUGa9cu1YBL68udJ%2FNzLzTuthJoztgMjvE4GqrB%2FnuDXbOMxaaI3xwXQkDbif354v1VdTFrgT2n1JF4zFKNiMDpeYzFBiICPpr5OE3eI3yGYX3tMxpvqCwTQl0uWMfuop%2B%2FbnUN7lTqvb0iUPZO0wGcSQ0N7z9A3cSOPCNGkP07OLrmUpwMokUpWWMZWQan8ge5mjD8aZn1OWdNJ2&X-Amz-Signature=50a31e3f991e2508f86be8e451dc9ac306a370ea3f6eec45854f54076d3fa8ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

