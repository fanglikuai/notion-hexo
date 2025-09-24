---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAC2JUE6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYosZC9DhZlz7bnqjT%2BH%2FXLG4b3QrfYcOjUIfR8jnksAiEA5OJ%2BDtPvRvTsxiRwfP76xE6%2FsN0vzno5wp2hWYBcM7Uq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDOyOapd%2BvZCIv1gS4ircA4g7mpfIioWHzhu4YEJbtvTx2igcHzW%2FksmwJaZ2bo2NgYLr6wo7z7spBWbn72CSWGXh1Rf9Ur%2BNz6AXKFuQFNL1960y8p2Oe8jhXgJpGAG1njnw9LKOeOaeziNFJZHJd3YTZVS8eMWBBMMK0MPh9zylz6%2FcDtjJcBzyEGSE9Dbdcam0fX%2Bth3DgQdVjozkB8SqldtX4gR%2FX4POOe2H3mMEJFXIEneGB6XCgrXY9Gvp4MKfwtLbiyZOk6xI434ZaydCDS6NLChIj1hm86Rx129Nao7z5n83FTYNfYZiSWF3EEyG9fc9XyuLs1QS1x7cfWoYIaiLuWlJgAPHncIfgRu5dwWc1izBdoua27VEcl84zx6P%2FTUsprOBeMJdjWNf4r5zyR2seXzrDRLTCDjpceyVvPtMNinmFt0%2Fzbfp21oJyUsNq6rvBqV6pUzQG3Zhjb5fNd0kax9lPU33skQwuml%2FCncWD%2F2DIZcqwOCJDadmK0KlSOZGjtoSxsoeAVMzl3LOvfGewOcy1ih%2BjFP5yeBOwg8TkZ%2FXAAePYiSVPjc144bozF99o63jG9s7pZ%2F5eT7OigKf43FxGzW6BYD3nbeIn9ySmLqI1H%2FJy02wX8tcJx2buAb9EmU1BVX3CMIGIzsYGOqUBea%2BvgRtcStasRqekygQp6q1kSxaatM92jjyGpvkQrFjDS9h68YtShM8mqnd%2FKp31Yp%2Fbs86GNjE55VGe2YWAvPzp0%2F25vtawpRyJFb%2BcYiCRQu6%2F3KheOKeoP0eO6ayGqjtFKdVXgarthCnkuzW7ogJmZ5J9QvwW%2BTNDHSVQ3e8Vx1XxegKSF%2F3NMxM9WJ8LVGBpSOnD%2BnG3JiH5yMpLY6SzTzE6&X-Amz-Signature=4ee8f04951196d6410cfc8b8f37edd8430828b48906d6e0b0e0ad740eda032c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

