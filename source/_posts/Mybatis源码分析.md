---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULS3Z7P4%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQDn4tRr1Uosmg7TV2Csio76OeJ9R0jNd1AVy0N830UNNgIgTui7dyX33rFgjhUZsKawEmbXr9b7bU3GtFSnp3UVTvsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDC1%2BoJpwgeGw5EKEFSrcA8KmvUZJBbM4qp%2Byp7hjNpm5%2BVHVxCA2Ynj59PH42keRaqTbzbuVXf%2Bekf0J24fj%2FK7SszbG39zoItXbMUH%2BsOS1x1s1WRc3LHyQAjI9og1wRYJbLMr5r7B7Ut6cGKfZEYXL9rfhNSyH%2FAynrqf5tcdYeZQ4kaZf9kO4%2F0p7qlFeBkCuOoAfch8YScO8VsDT%2BSYc5QBNuFtwZij9goJHZL6NHXcwIovIrAOLa%2Bxp8GUOOToAWQ2o7G3H8OMNBZNoDy0L4ve0IigxsUUMtFCA7%2B0iUKE%2B84kgvRP7pYzvOE3YOEzHYPpLyN449hpt0qNlss%2Ba2DdDMXLVJpj1iyohLzHzEp9MZ2BWlhZKdB2pKzyB4gR1jZezYxjpldpyGUOFJaOfrEdJyP49Oq8EieHkdPMR6P57lCTC57F6ztMmZ%2FC3hQDYqMXLnLDJn3jDBulG9ye3cgF991c9NAKkjQCw5VQ6YzMs5RfFcmPXDaApe7XhkJS3wtlyfr2bki%2B3yiWVivWd8PIPcMQUOf4MKixxy0CcUHp%2FAI5PuRWI43eYbhlzEEo3XK%2FNIC8s2yYGqgXg91fUgD9HfyzEjPAxmq8egObd%2BnGoePptj80HfXmR2UjzoAGpdAaZxPnCNvOkMMrEqscGOqUBn2d0Du%2FynyL8FliaXJnA4nor5TZ2BlNuwE2mef1v4uOXIl7S4sFMptazOnKY8eeyR5KoDZ36sbR8zBMn2PqKlGoClSIk8t4DvYGiIwdNSyUfkcVtupl2ydNmSjjm6auu%2FcR%2FGaF5T4Ug3nVjgpU1kOznGf0mO47mSh0Eg0pgip9aW%2BZCVzn3UZI10V24yha7lKIOeMPWAlokRcyAYoXBKD3SEIms&X-Amz-Signature=1819ff7654990c9f0a9f618dfc0dac421659e9e9be94ea50a5aab85158b730ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

