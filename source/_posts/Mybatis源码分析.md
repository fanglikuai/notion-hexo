---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWDOTGXQ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCSbWYY7LC5vE%2FKtNTvNbOpP4qyRCPryRlEFDZehXmsAAIgUT5EEwIDAdBDkF4Yy8CVoj00h7R3pp6lgQMAciN%2BYgwqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAQk0EEI0O9sOPBNdyrcA0sgdO%2FNfANd95dAKpHPeNBnZx6C0Hob9N%2Fp%2FYexJxTo5MUhjBA%2BsmVduswCo9DWZFkHlkO1HRvvZs2OZIS6nfJt4q%2FOz6yTPDxi6pCvE6nhrOQEigl9YYv34YoIQQbg7J3Bw9NNG0%2BeopLH9byVdqKu4jSI%2Flg7xuL1YMvI59PsoMwTfhKmLrrp815M7Q6kH3Il5A9DD53rx4kytzzztTLwv%2B5etXoV9WS9t%2ByjXZFw6bX46Vzv%2B3LMX8lfxc0mouihyANPiVqAEXWNvQxpFwj7b%2Bh7Ufg92yhDZ%2FRmGP9%2FMTYrHCWo945VwGDhHL9uVr0Jeb7Gc%2F8Jt3bMZNaWVHGMiY2LXP7bpWBVZlTdhqf7hheDfg65TJUjgByYRAy93kiDNZGYIFJelo1BzLVbWF75bruOdegt%2Bwx4X81CRN8ieK65Clj3yoh6i%2BxUnsB0v9vZbYbxPkqqaHIAdlZUQH08SR%2FkGNpkDQ8BIvqc5srdm0lxjTkItwFFpsho6JP5lB1X3WmBe64SDHtVLEdpBOQIPxbuM5Nr6GsgCNCo0Dki%2FleCEOxIIFMPgG3Ccr8Xj1I6fODf4ulnbNen8bpN5SIW4vUJogCQslD2vaDRPigsTs95K9W7oBsFnozTMLi9rsYGOqUB8dVOSdhDzSrn9p87CsOoQ5WsQ5kQp10PsZ%2FTsi2Fq%2FNMyFphVmJfhv2rNwJ4RhxkjeQbChVAWLmzNYEE8aqLgW19T2whLnpWy2nW%2FFnbIzhIuRIIj33Bl4tGlH%2FBGdXzODy59H7bymeetCmeWHaYud%2FAPCmfGqWBzsZb4TKld6ZAJ10Y%2Bhz00KIGUtz0kVWj3p4NwcpNfoAESTYmLyTpK5GkpDh2&X-Amz-Signature=a24594baf59ab04573ddece0b0f26c807928fd33463362fca0f2cd842214f1ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

