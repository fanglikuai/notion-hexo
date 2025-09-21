---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIKVY5HG%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T080110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCusF4A8DahVLko8qltw66InqYDeIaoAGIWxgf6iT0t%2BAIgVUxgMHlNHbRUqos%2FLDzn7Qo0GC1jg7%2BStO5tBAMeStYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB7eHeT4wf71YDDCkCrcA6deQAlvS8s%2BmjyGYM3etwBITw5ORyhMsoJTnXuQ%2Bw7sbNRlp%2BG1EMPYgP%2BmNCGxyzfdoY22%2B0bLez0yckFDNO2mQw%2B%2FoaAMEWopeaJtrfr2hEsdeCX8TfVYe%2BCZ8fCsQifScxeb6ox7%2Bu6nq%2BtJXFoCQZ97wxK4jkaJJ6dd%2BWkXI111yitr1kNz7hMfNtEkXf%2BIjEP7xALIdG5Oute2p6E%2BBh4%2BkN8wnqiyYgXFN7utWquy%2BksmYlOExWcqtDVDjJWGh%2BSUsSZMKkU5yH0HC3eKlRUgDvCWTlj4rXH2hcXZn4r%2BJmkBfcGizG9o9xMPReRfy1FZrcTo795iloDvrnB7UkQ4nZlVXUY1fi6CxG1pQDi8HnKYoCoHg5T4jboeRxooZnWjYpaDeqJ%2FPhR5Fed%2B8IsF1Q8%2BzNjFXWbaNC8VAdlTLEBeB4ZRr9qgyNRlGf0qoctTcypR5h0sk%2Fmz%2BZTzRsnjLfyeFxwC1Khcyp223%2BrkNMZh0lt3vxtdf92gX5JfA1kKJ4a4G53Zqt9LxSZSashXw5k6Zr7rn5bpVUMUprRl4XlxBSmI3qEuVbYkB5XvghZG8Kg%2BcXQ85uJ3Bt9G0nluSy%2FTEJkTGf9Qo8Aw0YA0iIR9AW6QjreQMN%2F%2BvcYGOqUBF9Q9L5WpAIkGhXkTbhubjVbK6%2BUgEBm4Rw5B9YzWSfSYO2ne485bWFVb2UWqhhLuo8qWY5UobFHav3XeAUC5PXjzS1iK43cCjdYh%2BojUUxXqT42NdNI6FzHA%2Fs2gaasnKqISU6dzGqTv9VpdQpA8NliQUkX%2FhVGRzXkRGe13RQntUHklM1o1x3AhnMVDxKHcYY2gr9AtOhf0fJyF5jOOsCS%2FXebT&X-Amz-Signature=b0ece78db3f51667f1946faff540b0a9f7400a874d64869cafbcbc6f41279edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

