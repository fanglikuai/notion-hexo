---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666CHKNZF%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1udryz702oVhjMPeYiSXdl3RacXd%2B6oeuiPeckYORYAiEAy9OtTswi%2FEVbXum9qCGXfrlhoYzpdtPpQX295cpylDgq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDL9sOxklxSRalY6q%2BSrcA33%2B4Wzxiik9m0d5YO%2FQczEzTR1oaqs0LpLBDmAmvaCPjg8XIXVgDx%2FZIaZqx4Oor3gK2S93ZNs6iBJ4j4yPdl8Ou5rFKQPwSAMzL0SZSO7IwhsvRVse7tuVJPzQPlQ5PLIICb6JrmqXQCAasF5Uy%2FAtfD4hyl9LDgyiwVB5ZVl7U9Sb4UY3hfbuIMWwrvjr9PoHIZwFGeu4jB8J1sHereQaZvokWkGsLeRh21N%2BNXvyNf5amr42ft1uCzIpu2dwQb6T8waSP1aLgB4GPJriUVD5XrR10xq1jhZ9qNylghKLOJEYaY2DiiN9F7GJ6uwnlyEHZrtuMchQLfOkrK6yhE7As%2FbUSqE1zxaBQd7BILjrDZnwN8oIxY2ylFx4JMMubrlNiwVXfIEppRt2w3xiT9yEGXp3j7aUnBCqe1%2BmykUisJKDEvM1ZMcIgrFAVuRdrWLTsXI86t6iW9dxGRQcBKaTr346Ime0%2Fuzjjl48t4jEvMFtEdiWPU2RpOcecK%2BeB0vbCaoY415MHrWrbGYInB0ULvQSna9QmP8Y9Z38QlexDasjqOZTnA78fKcdOWIcwoBcZFfpD36B1zvDhbRb%2F8tr%2F0crrZ6JssLssvlDJE8LQhmdHzGRRmf9vXROMMKQ38gGOqUBn3aMSqHA3%2B7ZIoevfzkcoLfA7J3gBtwU%2BuyKeFZEzXfBe5MqPiYrtoTtjaa0%2FRD%2FxGCmipmEqApAa5WEZg%2BBdbjHrC%2BE%2FkMWCRVaWGikKbeM4%2BvimTQbTxaSeLFOMf1DOC34lXAqY259Swyr7sJrE34RXjU7g%2B1tQbA9lrSTIcm9piyjV8oe3TC6ByT1zs%2Bol3Ks1FEYPqVuBYWqNXKO7LdQnmKs&X-Amz-Signature=e434edfdc06bd220eb10b7bc2699d574d9925930c64dcafb457d144e9b37f4c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

