---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LJLY6HC%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCnaC7M%2FzVMVZJCIt0ID78HStx0kDZYb4HhSLlcoxtaVAIgeSm5ooSegQ5isRhAC8w3kRc2spjLgmpK9o06gJrQy4wqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIaJygwyLxEfXhrP%2BircA7LyYrn86FyM%2FbT5l1Sn%2Fbu5YVX%2BVEGvhioixYQbJd%2BMNSg7T1p7nWA0NjN09N8%2FyKgUySbfk14qvGKWON8tu2s%2FWYWXCx%2B0ZQi7GMJGH8mbNK%2BdPZcbW3sU4DooHbXjRvUmyt7Xgvvmi60LNgZumHSU2zwxst9DFSdnqXXcUgjEjfH0ubQyLL3AMrxormSKf3Ae3L0J5HyT6yVV4TT%2FtOkq%2F5OFCusUVs%2FcyrfdYrDsgGxTr6Of6HZUK5yRIgQMrKoaCBVbIv7lR%2BR1mcAnHhfA9KU98CXJRTmvRV1d8OFRh6JrJaA5ar06VrJzN0bGJR2J6u%2FPG5qeSAsB8TyL%2BEf5PXaC5RBALiIyOxEvLHj%2F%2Fsprf4N6vJIpukUOIwQiMWxidT%2BLnJM%2F3O78Mol5ZdAN0jrJYo2VBg0%2FwLQjk2XNAkANPEggdKNjmbRcpY4qDywgiiaXjUPpqt45G0%2FLA%2BkyoFul3C1PJi3vMubCXzVHofHiwiVfsCPhYyk9MOuxIO36K9l%2FXpGU39wpaffWSbyKJxjSXoEgWysNNxu%2BA5GtsMRoK0HFscty%2BTghjxaG03g%2FugiTmZKVupAcUXTpfZVNF6yZcM3oykyt9qMzWvhyfv94HZ3N9rSjxoMNMIOS2ccGOqUBwEqws0MKhLFk2HxhREDqb1wsVAMQg24mDznJ8OWvIegYVI%2BtCwKw4O7wZuwKrsKoDebqfVov7xuU7j%2FgCL63q7r7VbofslVPPu6pbbqi7t%2FaRqhHSnDfnQ22xisWWHo7m%2Bv0S13%2BEqLP6uveqAbzJ0D9iL5xFPxClivbYbK48e32FSFADt2izY6dXXKpY%2B8CQtZ0iMa7XGw7h9u%2BlZ%2BSA3B%2ForUb&X-Amz-Signature=8e315ec75c4431edd7facb27ec66c1a026532864eb7562a045adbf61970096c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

