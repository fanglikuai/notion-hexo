---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMATCXBR%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BJ3Vsfe4fdL%2BiJmWTV%2B38qERDO936SVuSkDkmyTxp0QIhAOqqHuSJ6udflUeiFOrqqBqbCoc64ZBVDIxJrZYR9n4bKv8DCHQQABoMNjM3NDIzMTgzODA1Igx%2Bd9yroh4qOuD3%2FcAq3AMus6jNpPDU%2BuEHZ0NGOoRLax1aTGhOT7r%2F6AF4%2FvtOClJf%2FFEnel45Qun2JvAnDivV%2F2EaNQDNFobEf7O%2FwkfESBoRaAw8qU8EC6Js1zU1hkDA2jaRnHcLL0sxWeD4bf5DR8VYsmDLmdHgbAMuiYkwXd6xmJR1xfCSbR2SAa9LvMaGvxM9i7g4qeEz8cbK0vv9XRWxyoGZY1ixqRvrzoMwWsPw7W8s3ALymw3R0eEbR04h4o3K1G6oQivqDSZKJlMVEKAq%2BjMJmD1PAH58S6MP0qCfryWbgulYxdgsLEFKUy38pDCCVRPQXou5nM02anXFn1jHCtaGYyp8UVY8%2F4WmUsOwIe9aoX%2BMlIoc87Lr%2Bmkp1BxGad9Kel4Q18i3wHQlNJV61XXW%2BArAXKklOr6BOAyFm4TxUqx%2BhbWdMXFp9JNqC46IB1dF17zVXxWrYzjMeffLRdvsdLI8E6qckbQ5F7jK%2FXKoE8AMoHwAhH8kijTaphXO5qBfsQdtbU2nXR6AqeFVE%2FAGHJeXt5zyDH9RYnk9aGqDgSuIkppDX3CnCLPHJI5Jphv3xvgPiibwiZHYT4wEvlwVF5h4AiCYRGPMFhc%2BgIpkEsllqOZ7lDCYBeu0Bb8YJCzL884t%2FzDA1%2FLHBjqkAeuQCx4X8dOvLl4kE4223CbIIqUPSCDmkBiMfPiqObknoE4qXOeyR3d4M%2F6%2FfX8Rh8Zk20liB74g2M7Kx78cDVRIt2ZZxPaVB3hAuAUcOU3rwaZA3ANIi2FYrXTC4x2%2FlVxadday1h9i1VrkRgtLmJafAdT0ZbN64RUni4ElWMxLDyvws52%2BqQq7D6wSWJUxIl3R5XZlggpk9UEv0EHdEaJtO4X1&X-Amz-Signature=d5d2cf147f4b39eb4639917b63b96e4125d36ad4664cf0d54111853276e9e879&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

