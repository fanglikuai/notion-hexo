---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQOP3FUD%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOugAc%2BPVDtqtU4yF3mrq2z%2F%2BkiB1fY%2F19kMlGTGgM6gIgE0cJx0bq6LXtvLIYxw64xtvAQlSOWDF1fVWiYOclvnUq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDA0BFuPaUWo%2BBTTE8ircAyWrIRB2QvwYqyJ1l3qZi4ENekcD7saIwFURewWKi7Nom%2Bo1tUA9syFUZhXps3qtuslWqKqHr5oqRX4tDnuwXveBQn3s8EiKNJUk%2BNJYomnLu2mA49BVYhzm9Fi2AGHAUOEEL%2FPSwY5ScK3jexVTPV98hlOhCFlRkyz%2Buoqht5MLH6rrUXgLAGxGlP8W%2FPEp2ubl7GHw0lAhi4doGxF1n38woSRLf%2BjXVSrOr8UVXcQWjUb60nDRuJ%2BILTB5aA67VgFa05Ztlrhz9x%2F78b4RM98jCJN43crE43YL5llkOqiB3K7F6jaqdD2QSWY43GH%2BWUbt9MVxkT%2Fun13nPraNjKCjHdtJQOse7mU92b1oNArWvUDF35TGM65EEDlFOSm%2Fk5FEcTeBYw8hTMvyu20ECDiaWACgz1%2FF0JZ8glMbTE6sFnuMhVquhDD%2FOJd74m7XQShfdh3eSQUF%2FP1xfxGlnSPJJCogbFKNWstaXCo1%2BfpQFNbTC54DjFsTWBjWjFZwfN5p1qOCe71EL2Nqb5uPDlkz9ESQulBwHPwSWMUGXX5piPel0fO31NU5oeDZs2DJZAdlHpxZspjYQi18xbCfFgahK8PqdOvsNwU5tKtRIF4edAYNuDgamqikKa6nMObo0cYGOqUB0iYhLKfMEGTd1mCTxPtE75rbfuETB523Q%2F9PoUfbbLY1C0jhJxC0RZQ0QzohqY9oYXaKiMZ3G3udR60JUkdPfxja9XbSlHI5Q8OBGJ2u9e397GzLlx0Vwp88BQcMTTSMcDwlYDT89llHY0tx6CaDDJR6xNHgRV3GPyzSJFyxIvZpNF5wSZ2T1vB39pvE2g3t%2Fg6ZWqnFFlCD1Ti922uEu4cr%2BM32&X-Amz-Signature=6fc41c84dc9b0c3247c223ca010737b2a4069b614bcdf9ddfbe12b673705b05f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

