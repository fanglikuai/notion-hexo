---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7P7MIU2%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDevQopGW%2FVTyhyPNukNiBBel1A0%2BEmlWbccgSXaiBOaAIhANGCpD3xi2FKXKjeZqqCJykP5cAbK8%2FrQFiJDa3qbLfwKv8DCDIQABoMNjM3NDIzMTgzODA1IgyQVM4ctXgzY7oPx2sq3ANgU%2BiHy5kq3CtuRjKFtYPtcGMX8Vq3e9vuQXGjEa0IFsZiX03REdeWyBbNL01OHrItVW9BbGOsloiZT7Lq3Nf2soiEkIpGlUUtcwbm0N6e2mQ3zIxaIh6KNX%2BvVxgqt3ixxqppIAgdQXZzRfZxudjDnv2OTJDXaQFbKXpxUuJWkLDAEdwLmFQSqbIXxRfM2gkzJbH9KjWwxXxSXOySKDP%2BJp3dBvjAaxhmyvO8lYErWzBtAWo%2FdE3Ch4sOfynS%2FcDRX7hIFgR86Uw%2Fzp3kzaHENywGuZco6w3cZWdeEkpf%2BJ1HeasCAPw1j6M%2BZ5Kmx%2F0Ah%2BHYH7y2oR8MOtQLWVY74XHU9LkkrZFIxapbf57UDFwFRDFAI%2BLP1Kfdk0MScQibp7KFXGTJKjV6LMhn5f6YvHDoacOgbfUA2Y%2BrBo70WSaHVimrb%2FzqIOz5fwYHsq2p9L6D17fb2JHb1RYLt%2BqI7Wy3eg83aTHs9nj7DIeXPXMrd3OyqnQoCExg73ykbYeH0npw1PFPh5qiS0QbgMzochbSkjL7edEPGOjinrKqzwgCsOLWbHmR0X4qX4bAb51Pf2rcRz5XLhzKP9mzeBl%2BgCO%2FMdbYUjvr6s5gr5zN6C%2BuKT1S7FvJ%2Fm%2BkxTDv%2BJjIBjqkAXyfB7%2FOMFj10wqXS6wPPj26SoeIm0cPBrdRdE0Sa6HPpAhxQELjuZ9OyMMTfeImH0kglqlOGfD%2FfGKQpc3xYZscjEpKiTy0AjJoIo8q5gMr15%2FU9VDaUs1iSkzl9vrdpUluqtWB0ue1zPU3wactlipXzKe1QC7676wSe1lZzmMxeCX2uRDkPxbmhYv8%2BfA9HZmpak3ZlV%2FgCgU0ZH5id%2FkG0yiS&X-Amz-Signature=e34db70fc902bf4b50803f0da14e9d61da84921bd0dc89f4ac3985e54be2b432&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

