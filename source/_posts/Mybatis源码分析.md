---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EMVGAG5%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T150108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHZlL9M7ELKHyqsLCwXXNvbxX%2BGVX%2B%2BwuIG%2Ff5LtXO%2FEAiAm7gVz0uLvwN%2F2webRuiTMVAQi2nZLeKxV8jn%2FSjcvCir%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMg9pQM8CvnZS7s6oYKtwDHr2iVCbXMbCKq30ztH7RZJy%2FOodqgk9jM5gQYpmghhbNKRWcvtVrAK71xVt77IP8whI7BBKtMwGq7gsUjpxIOAkewh7yKEeKiwQ%2BhCXTsv3NoIOrodtsE0sCdOhRtDxC4ivuPg%2BjUUTwiZzG1MzWI637wQ%2B4KFU4CQs66DUAAp95o2agPMPJP%2Bk%2FdiLxPblsk7w3inTVEaK%2BCs3ARSKbc1yQZ0SIGELY7IiP%2F%2Bb5jl1DKlpY75xenE5sAYOgA3LATWQ6BZWSZnTjycQ7%2B7u6vuodf7gfhO0WqN2WRCINJ%2Bfu6IDly0bT33D8hzrpVZSgToQsQTNoS1fLGOXSWmrNxUwI6th6pNGoZyfPNZ9Ir3Jsm3D070P6IsZr7%2B1hg8w3LtbJx%2B%2F2XugKbpaleyP%2FuBEiIXhyF2%2BCZNu9t2wbd8juS8EFfMpTAarxDMQ2KReEIIkZA3eyF7rCCkvsSVOwiCUQ6lhnyLAQdhMB4nxfoaQ9sI5TRYWuNtKZ7%2BEaY3fOJSZzYybpfTWWhtF2ZH1YDrdNm0oxV0Y3txo7IBsPRaJ3i8S6RMk9NvM0SJEDy7A%2FLIISJSzM1JmXtopXgq6%2BUpC5SLp%2F9Hc2PJBaM3h2md7WJodxe%2BLr52Bl%2Bbcwu9fyxwY6pgEkpMq05Ky2H5qzfMWsLOPbJr%2BdpVNPmFrpG%2FlIdAmMkd48W2Z8%2FyqCiyhUo7i4jc5YcwbUV3%2BYJQSA%2FC7fVCv9U4yzdNwJ5Re4lDAh9KHrmePzFaBbhZyB87Qi8Seeee1QsYt%2Fx4vE3%2B%2BEeNAVqjXKxFCLczy%2BnnDz2zR1PurwLVjbx86d9SzCM6%2BaeB1GxZBqXZWfFuD%2F2LdYti1wkMKuYAX1oJyT&X-Amz-Signature=49fd019ccc0219f42679a7e9cb66a809840033cf0321f6f6b9b1d05e6c1bc479&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

