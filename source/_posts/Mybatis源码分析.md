---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IP7Y4JA%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCoDTS408ivHuZ07LKNPy5lYbTP6qCykVUU5QjDF82GLQIhAPW3VZHJauu0LYrMIdiPMSt41%2Be0CWvKp4CcfFhJw2TeKv8DCHoQABoMNjM3NDIzMTgzODA1Igwvpx%2FzOjf4V9pkL%2Fwq3APZzYYc3iE0d%2FBZTLNSxlaSyzyNH%2BhOWZQ3WuW8mMKH%2BXEGJLwyOBsQU6ziJcIy1JY5r1hLZWypjDYUJJ%2BPYTWrEoO3pqgt1qOaeooQqoekMh84LoGpEvAcmUNSI2%2B0GPLPoUL2sGnisgHXVF4ZEffYFUBbbcXGC9BmPWw5mh377vCbzzpUyYjglC99QDZgtWifezvmcW%2BoaVZAED2EYlk%2BJRsxxtgspO9nYG2a16HXUheZA4ICmwKVE3G6LXhimjji1xG%2FCHKGF2ErbUoNYoSwYlTUJ48SGswczqB%2BjkX5wNz0QDn5Q0qMt2gEG%2FaMsgyaauiucsNo3t1G%2B4iadRTsVVdNAsgVjkonYj2UwB9CNBcettlS2S2atmRQ1ysYa6X1ydpxjIIeXYeHLpmwf%2FMP1JT14%2BFzw3oEr8ZsdNdY1g5YUTGhBJhlqcXhS2qDCKbJAICZ55n%2F3%2FF8eL8GHxwYUZlUAIIMqOZXuGH7aPLqBvNLDqzZxdsb4HPzjFXhyOhF36nojiDBDUPmXSsYe2Rpk%2Bv%2BoQHntID8rQn7dSQm3AQaKjpKsPlgnFSZC6uGugMXw4iO20P36dh38ZQdrY9%2BBIJFyqOn9ztQbJawKZR9MktKsR9LlF43TggrZDDG36jIBjqkASMBXgUWuKsjnvwtf8ORh6yL0X50xoGBBko8O%2Fmy%2BSK7qDEvQtqepn8jelmRrTrmPJ1IBbunh0xaZGSi7u6mrxIp0kHf8%2F4%2BcR6Tck41DFy37mSbfcImWt5JzLwyi5EQp1FT9ucVv4SpywzC7PIDojm2%2Bm6IoVzMAwtoDK0KEANoBuRHeqAoVCWlBBLjkuS%2FARoiEt6QhOIezz2eq%2BOUIETsdo9T&X-Amz-Signature=deca7c98590f02b57d6bd883a6335d7a6863c71132b8a4cecacf66cace158885&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

