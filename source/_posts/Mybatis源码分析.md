---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQJWRFKS%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDysqupAJB35FtAxrn8ZgDAC6ej8wjvNGGPNl4HlINgmgIgafv6%2FzTC07Zm2fPCyKwxsI1N7XnOd0udMK%2Bbeap%2BNBgq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDCCuBTFxbFysILPS4yrcA1uliV2mxjkrbdfI%2BzQ325sSCEMUeR%2BsW%2FrDt1PZVuEdSE4jUquqBB6hqbUHwJThgPwgUQIOSFaWtjugA8Lx6vc9cZC%2Bi4Ixj24svOMIKjBIMzjpiyIYkHPfZMtPfeQRqq1u4KKIW3p%2FdY1qRNy%2BjriHAWcTWnvZQ4GG%2FyOvoisiE8xxiYRyR6dyzwazOFKtYXKjDWnWbF4PybuQScUwEzgGLU3j0xt5neruJEyFUH3A0P0XRjy0%2B7Pd08dK%2FG3tYXMkEZG2dbKXPNKDUmqo40Ki9hg6nXVPYJEg%2FkgrQHe%2FT0fszAjRA37p%2BZYt0gubIlNB1j9adm6ivWhpGefbkzheWZkN3hpAmasOX8VlN6lgnxdBjHdvP4siylEx9n%2Fm6n3Tpz8sJrUGy8kA1Pl6gIcgB9OyACMWyag9QdwE%2Bsrnqy4ChrnK1kb1brlZyibbuhS3Hc6sYaJi5Mez37t%2BmLHtGxsQyk2x6dKsLa5YSeMuRYdOWLcgzW1M1aLH9Ov22%2BVX1u%2FlOcfm9O85MoAMAdrsjMq%2B7zIBSKF0NDw5E%2F3GvVvwTRFEmBHTHFthMkO%2BSUsx%2FZSOUfiU%2B7pCdoqRAVDOVBawHzbzUw%2B%2FXYHrkHJ6YXF%2F%2B%2FSwXzLpM0pSMMbnzMYGOqUBt4L8oyNfSMkNqCUG55kcYQTkQP2QuADHfEQn%2FFiFTxLMQ6%2FnY%2FgYM1cUT0W%2FNL4e8FPZnCqG27FjrejiGsSdTuLruMtpCqW83Pu8AWK7uB7jcWT9Lj%2BYlv9We%2FHJWtrs6g%2FX9fjsM7Cp6ZSC4lFtIeeLjcgfjHDc%2FwLAsr4W%2FzuB8Bh%2BONIm1SNLbXFDgIyUEe8AOQkmkF%2Fc1XchzyTce8qDZJBP&X-Amz-Signature=cc17fabb16d474c9411cd4b2d6b88c207a063bab3b3194c69fb8be67832d951f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

