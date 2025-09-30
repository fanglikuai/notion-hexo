---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLT24GYK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJHMEUCIQCXTW7OrAWsMwGPe8lfJVo6Dy%2Fsk6nJeRURFiLXEEokUgIgVVTSgFlILecSWgmsfvndw6OVRb3zFteo9yMYzfvWZh8qiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNvAgAeMEO1cp9ryJircA%2BfA1w7AJcxYjFZqvlNNW%2FBs6F8kvbZEhKudjq22km9AVjd%2BZo1r9mJOYqlLD4zoKhjpPqZ6uy82C%2FuY9p8iET9TsVJYV54uNyDpxBuhTHCkhDhPDQn%2FGeXn%2FBJFb8P0ZGdk8UjMD14VZDh8eRnz3QBOsFx1ziRajFUMbylchA6BkVrj2gJZ6Hi2L2OdzaSdahOcmn43koqYYo83QqKdMe4bqQ2KeQMbQZ7mj1i6x%2BKJ2FXfAG9GPTfLN0qh%2F3skdiCHIGAYG3aEFyEPCDnQ%2FxtoFlBJw5Uy6Ub3FEKsKgIhquUqyc%2BxIfDObmJ5%2Bvm5SPHeO%2FLRD%2FhmMbQBLQakH18US9Z1abReFQRjyTM%2BtcfeQktVhL79skr2b85ZF05%2FvLu4Oogqrs4U2CkHaLaxPprt1mp4x2L%2FPPOSwXmWWNY69M%2BbM2xZQp2g2eQ9UUfF77JHzpnVe9bZtUkdaf%2B5LwjHARbF%2FfG8VgDLTo09XnPbWXTP%2F5WqaJGEEoijMh2aXOXMGEqJwfIfM24FANC8Dt42MXMuonYE%2BbT0V30XnvyB6J6JCDEg2L75fJCKRoKgjjekwpM7CGeEzlGCO1kfWCABAAz3LQ4YeCaTyrfg3VXMWT6ePTBs1EB2LjofMLuf8cYGOqUBzVCjlaL2I20giFaHrwsQP4t6MkjXdtOGMz8XfU8iwS%2Flxa8qJPSrMvAMMRfxHZutqfXd3OZRo5ztgCogiZvUmChkd0tsd6iCdkHAF5a7XNOOcUviGfEoHc8gJ8w5WEAMebTJ%2BpsSWDtb2rNiz2zJzbSsveLU7LSXBAAIg9Lenb5t6stmqvVxlBAn6qdW7gYfh60DaWWYd%2Bx%2FgopLeCHvw8ciJxmU&X-Amz-Signature=b05da4b24a0f3d0418e50f4eadecbe67797860a3573c786f2346039ae7cb7b79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

