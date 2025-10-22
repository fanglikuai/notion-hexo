---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653EQ4NUI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIDNCl1cgBQcczrZsQ%2F1D3MOrBSYMGQDjVV5OK4lnsNqXAiEA5iwqpyBmLvPaQSHjqmZrlvI6VJU9dxv%2B%2Fo9wtwZheOwq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAcaMlIUc2oVeMBInCrcAy9uZKDqCpfRgaeVTBybyXnD0DxwKZPphMZ90ATO96bf0wh1LOkaTlIDPXTMsJb62k5qEna64VdUq4uUBODA%2FW2Uhu310%2FjhL5o7OrhF%2BeSDSZsxUTAbKPskJze5QdSeVprhINFa8zyoJ40ZsSSE0dNcpx84WVNmmjuMxaPuZq9iUJzqqePr539UuPOk6uCGEOsY6ZuwY2JYhE26RrQ2xSWp%2FIpAgA9QA%2FPwebHIrL%2BDiiHFMSmOXkfh%2F18ZsRKBMWbyKalOS2Xki%2FVZ2YNwR%2BHCa8nRrOdFWAaPi1gpDQyXf9V6boVWN%2FSZZcFZv3IcPr1g4zng3W3XDrKUB972zHWV85D5Gfdi9yLvJPEBkdFmM5hH0%2Bkvq6SYx8oA8p5amWLXXbEPWKirWYj63DseMOnHOMEVTW%2Bv4PDYvp0eC1xugUSy5QZYzC8iyEoGKBKaVv5jhXSBwgk6lsLEGpAYuHqX2nCeZ7P4I128BuhB7vm3w0nsa39QntPgVyDuKJTpmhNq%2BrbWGNqCGy9qeZzJk0qTSSopVzAHfJ35QKPU4IEyvXkaCT1j9m6jbS9z3BsMcJcQHKstV71ZtDea3dDp5HIH6nzlWmcygwOODOD1WKvmDVQnq8wBUFW1K730MM%2BW5McGOqUBxKe5WcBjmNMf6EZXHHsjxJhb8bnF24lyI7mMKb7MJzkF5Y0SvDJ5a8FZkXZffvouqimGzpt925%2BbwWzlayI75g2m5GZYQebpCHI9EmVpzn0IKGW1Rv%2Be49G%2FJgYBg6JgEPw01fIPnBh%2BH4%2FNFXBWjxotrlsGz2Y3dak7B1KEm1K7AFJBujLCYawIdHDhUOGiYM3lTvqA3IGzbjlkByv1hECJCjMM&X-Amz-Signature=e4c93d96dcd1447e52e04c40291a885300aef6d20dd6392e28559f0288e22a1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

