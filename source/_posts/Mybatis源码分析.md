---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46645YQRRM6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T040038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIG3k6AROPt6wJyjn%2BeEkBo3qhiHZRgrhu%2FGRKc6meXKqAiEAhNar0DiODfMhQ3FTCZSDkDG36ktfE3XJlGFNwEQQzBcqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJspIDfRBV5Ls%2BlM8SrcA9PyL5L6FsbGw1GU6ezLbJm8SIfwqQppAuDGRTpLxRWj%2FRYnaY16vR0G0knTtxoNRLfhKvOh1N42HTtjBTfY0zAVH8g%2FAZJruCg993YG9GV6skCqPegyHLcDDu%2F%2FpErE10WCz2voeHVN%2Fq1XnlTHeJwRLeM44kbECf2WuJ5p3BsBvLmFgkQQpEYjD7Vor83GmA8r0Hx6IX7GQDzawvg9Qd95Av8lA2tanQWccEggMFOsApcP1KruG31VLcFm0KbdKjNQ%2BXb%2FjdTMjYZsKMD3rMl3IRxgZ%2F6wPrAvwMd1M0RMUFN%2BDGhhn63smz9cnqjAKlp3fGRaDLs2TbV6kan3W2hbuVm92E7Ci9HxZBuYrqivvO0xlBVvZnbxEiMOd2Kv4pWabCmfeMWXKM6i%2FaRaR%2FTVeN2ZK3KwhQBiiU4DwzU4mjGVdRkNC6%2FVmrj57IkGr9p9RWQDrh3%2BWIP6WMS6DBzfD617xgQvGnM8W0OVVSD47rHuYRKX%2F8KeSJnizYs6RgzucB0SM0XVOvhq3CFBTluQYP50KOonvS0xwAfegL8XlcrL35RkYpll9BUtw006AwKXvsaQet6vpD6EOKdkfH55BYoyPhZBeVUeYnxBY1Qa5toRfceG5V37gGuvMKTSqMYGOqUBi8CW%2F9mP1omrX1vXnnd1WKq1JMik6OhCY22fUGyydP8I%2BTMJod3BwldW6A4xJec4P2Dh%2Bcy0effE1fOSnXX%2B%2FdB4ibcnHgHjmak9CipKqOu1kFw18gARXyeh97dDmPSrckEKGf20ctCf6gbKClJeSlhk4yyw6PBYVXNzeH4fo2dqMbxvjpERdHBcBLf5Eb2reGZtIIHcdKEaOqDspsVA58VToAT6&X-Amz-Signature=2078379927fb7a1b965f793f1c999de0078e1ae7fd6a8a049ecf5911780838ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

