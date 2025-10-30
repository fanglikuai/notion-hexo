---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQKEIUZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T160205Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDjeM2dEJcAElGoK8aULNehqnz99qSribvbfj0rPZyXjAIgAoLjx44y8JvcGQl8v6E%2BJBJCd69tuUxHeM319F94IDoqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtZWu2oGl9NxikW8ircA33MvCMWvyPesI5Hq4G68psBcCJZiYnyY8QUlcSS4x3FGAYf7l25I4AS4WnyoLRXIkV3pjMhal5zTrPOQ3siaadDqDs%2FiWB0FnstXzpFmol5juQgl1ZvjEZeq2%2BoYD7vlBm6jwSAhPkh1%2B%2FW8fQhqw1pLGLyKaOYLD2gLjf9rBJgnqwNbRiqd1TrG%2FCO%2BR2%2Fz9XX5HGmEQMuw1kGSsKwqZWmF%2FlFJnyR%2Fc6Hw80GdMWoZvqiUADXRlgSzC7CpK92MS8rCaI2jjWska%2F8DTwN05gk%2FdJEmYhC6fzkDrX3SD%2B9RtC3pqkqLnhEYmLaoHPqCTBupDQtEreIrYDFfDitWKd6%2F41sr9nl0%2F3S2sf7Cyk8%2F7Y4Pcd8oM5IiG8DBvmtxZppF5Y1ndbrMbm4GudKJob3Oet%2FeHZiV5Tt32qiJJXIqSMJC1nXY5rQDcMQfOQMGk3VwaFj3JEh0rejU4ux5460PrT%2B2YzDQ2x2KUGuoiZjHSpKWuvVTMv1J0mI7jJg4H18YiNBtcHndhV0HgOlW8tucoW112oa4mF5y%2BYMFaU7n4AB5nLgdrGp2TeCIqXJJqb0P8n16kxrnLRHTAFeNDQuzg2qmY6lXt%2FGe8BTcnJeFR2HRVbMczqhoY%2F6MLv9jcgGOqUBxDvkQsmY9Mm4URlcW%2BFcn%2F2TttUiHGx35YGufuZ%2B2P%2BNJDN5LUimiUldejS6YFuAa0CmF3tekYYy0B3qeUSr0dNMiBA5%2BjPhT8vBvHY0j0Ybm7WMZ6LJymiZVgCtIETqqu3X0cQ07pLYj8SKoc2PuPaZeKQtAMkrkDEus7sP9IyXp7nqYIFZo8iIi4wH2MnhYkidkHlpeNEDYw10WSMaw4HfA7Uv&X-Amz-Signature=421c38db98ee0382a940788ca8def73073f6f7f90a236604373d18ef8adaf183&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

