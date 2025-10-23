---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y4BG73BG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDEIPDxYdjBzDYf93c0DTunpdySXchD9KfZNs6kFlIuEgIgagwa5U9zaaYUEgEACzsG78x6pZn0UjpqDQD3WeahkEgq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDL5chW7FiPeejZ151SrcA4zWSYzRAC9A5dACtbG4%2FNHE9ELtkx%2BJH3Q0ntv8WKcgRt4MCS6dSOkt8yu74UY7KsMQPK7dq9kq%2BCySTcjmmJK8d9hgjeM9YPWyR4JS6t77J23tHM1ZFZwcFJuO%2F2EAvned9PMsqmESvGFJq0G7XBsCk9ZYmWV3Em0jVbE7Hh87yNtx6gj6WSc%2FGES%2B0MCSl%2FOZfjECbObZL6Vu2%2FVyiEvnbiF5e4LeA%2FOX8XNi6EyxiwXQlOItZTyLCPsmV%2F8oY6MDAg0pFGKZ%2FsG0xxMTWf7jNfbgkZJy8mw3GftHImNcmdC8JHvTpJ5unaMgDB5KZuCbQZlqAA%2BePhtGgm5agqIg59cH5DEty%2FZVtEo5EZi%2BhsS0UMalaps3ZWWbCDG0OHhz7mb%2FFLw4RhxTxcmiS%2FMow%2FX24Z%2FZwtATNayNtKTzyrbPeY8oMf%2BcWO4ZBCphESSAApbbIB0qpnybhOuEbRMkr6sUt%2FSU6e5DQ82PsgHfQoX2ScusniUMhy333kwVwzqlDZHtXD5abg%2F4FJVZlODzSsgl7nLSyDdEBMql3%2Br4qd9xO4UVNmk%2FuJz69NrsvU70SbkLx3TvF%2F76XxnTWsUCs%2FRPOUG18fgX8Ia3GpYHJW6dvxARx%2FrQ4H4xMMf85ccGOqUBSYgSTODlb%2BEhs865vDLtaYBuiLWHir0i4SO6ztMC4ku13OIFSRgKua2VSFp69Kp2qAUf%2B2WYHfdA7%2BCiQZ%2Be2B5kd7yqKqvfq1pdI8MLPPpylKRSuakzNV93SG6C59PjWjh2SS1BEAgKJ6ysGLLaUu0vjVt3IyfpeXtuKcvV9h7ovN6i6hnturiu%2FjcT1NZKfy2J%2F%2BmJLKikY%2FnQd%2BAI6a0gvflm&X-Amz-Signature=b313490cab3df23bbb90f30d4500294dbbeb3056908c4f0826f59935c632d98a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

