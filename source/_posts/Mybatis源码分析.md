---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622XU7OF6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDoGS22BoBFajOXthKI7VzKs3YXKnhk7UMgkKHSTfxc5gIhAPcaChQ1f5uIdT2yMg1dMFCBPwP1hMhO1gSB3EWrivFbKv8DCFEQABoMNjM3NDIzMTgzODA1Igy0kpMDIndqvC6uU%2Fwq3ANSMcw0G0mWYauzql40%2FbHdndEU%2BQd9CQExyV4p6tLnhRgIs9KuhYrWBalkgN9FNSkpLsODcFYRMrjQUd3%2BA34HXYFyQJnBOpc4WKdYadCxVBD0V%2FA3j%2FtDWt4a8vTbD%2FZ90PFICMCNeIYhuJOonUixgEQCpeJzVIHlwMMhjIURdwZGl0GUzj1X4KplO7iJVajWA57%2FAAA%2BYCoo0lJbQTYzbXVDZXs7FDd2RWuK6NZvvnEKwMW9Ew287lH4xlBL9THM9ZTkyoztgdRWehzkn7F%2BB%2B5tNvCJIR%2FofUi2dY%2FzXTdQmsNhmMGAf3szED2j%2B2BvkRl%2BZyemUrIPIVxJ8B54OrokXOqHWmnMN4gfQPnkIevhO6f7NYun2Xs4k5xEZXzxh0topxKvJUyCDzbUF4rLwfHVfaqrYklNRW%2F%2Fg0wwZr98mSOwbb1aoJyQVSNw8qfxGFUq9YFVBqkIE0akGVfgAeZ5iGuuv8chXQAlwy0a1tqHYn2gHV17b2JuX%2BGVAW5FoD1%2BkOueDb%2BTqljrxP8krxfCy2u9WxJO%2FIT5YiSxJNA3caUqpzdLfAt1W1ITIplvGNqxW0lKkT2nLJI9XIUIQsDufHQFnCL%2F20yRtJZLi5JrcXGvJcTY0E68TDCS0IHHBjqkAVV3jlpp7wjbc08wDn76rkBPDYYh1REw1RGnG%2B5GHdDJrsmM6ZKdt3wDNY3AtLopwh0GBw4%2FdWKJAJGXr%2Fv02d14N0ml%2Fbp3SfEAmZGMSSHwYfG1alHn3u7yzzBt6ZjM92UNAH0XyEWlSpoGLsmVzDNZYkplCyp6aHuO3LBEolgJmzJibfijdTl6j5O733mS%2BECuzYIT%2FNCnnqsbtHwvxTJmhM1Q&X-Amz-Signature=6d9f348fcc94d9c80fab559af42f82f9dc0c9c60050c5d0b5f00d4d43856daeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

