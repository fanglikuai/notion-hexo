---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKXUEZOS%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSuBuAq%2B1y4uoksjUsXT0UHPI00XLExRB13k1Hfa6R%2FQIgZpmmH4hIUIF4SVuGbrV4jEpBeEWmUnlIdKpz1TzC%2FTwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGvbTTYYcwOQGtjodircA4Ps3CgFNJTiTc5O4mDhLD7TwKzJcfdrYf3x6gMKwsLci7dEp9riLWSFd78jAu6AkR8ZsaV9ok2XQt7W5Mo4A5%2Fh3PTlfe9oEpN0eHr7ySl0r4OjVy%2F30g1My9%2Fw7hKSHw1eg3NDx8sU5B21Ih7ihSxq4YjXvihR%2FiIjDSDKb5%2B7WOhNdv0LhZ3INpsNWppkXTXC0WQNiMYcO18lVxrbviUcSaJPXkOLzcZuJNDYOmyFLXgVl2yDvLUU1FNQ4A9HulqbVFhS7NhjrpUUgzW6PpD%2Bpd7%2Ff5uiccf%2F0qJkiKLP6aqd3aB%2BCTa2N03tVF0i6kd0%2BRMFjRYnOQ%2Bd1bJwd2R9n2DF6oPxSIJKVVq7RwcqXcLJyhFfkuTpDvlV%2Fw7iFWMVeTttI8E30bGRqTk%2B%2FMqF2Z5Ahw5lU35luS4Mdw3lwTkKFqrrswF5qngyuF8vS5Wab0mB9YwVCEpn5Pi0oFm1JlJsNRN%2F7%2BFTNUVgPBhjUkpKbUVUryylg4%2FOl%2FpuoZ9zUOiQgt6wQ%2BRVs5Ld0nfLt5S6z4Fm5xS4%2FZ47HAUwbzJP2wxTznjjimJipijYKoRdYouguEPYIZTuUgndckRwJguDpxeeKFrgRFw08BjsP%2BwYGDeEBQ8zhYrKMI3gwcYGOqUBgimtMW1v7eos0SfP8%2BAA6mzW4qvihKWghtEZd4nzkj3WKX%2BW%2B8kXvBJOIixyChH%2BgUoBDObhkzku5G6cn%2BrbLkJByRUUckjwcYiAsPkloGn8ZhFu1HBmojx5qzLZCqwyji%2FFLGAU4tKI5ZJDUf9342rywFg8BYC0mnUnkg8ZJZppWSLVMBQwHFD9OYlqcVP1fwh4ShGqXfQm3r1d5o5dckqqx8wy&X-Amz-Signature=505638419b3c5be4a79b709ee842a6ea0d9826c9b2989077034ffe646869cc58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

