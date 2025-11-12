---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNRQKI6K%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIFjp%2Bic6SNt65bCR0DaME0DS5fvVEJPXNyzSinIA4VDRAiEAm3rdr9S%2FkBrQ%2B40HY%2FGWpeHPq977btqyhjRelCPh5wAq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDB6OTvD9%2BrFIO1DW4SrcA12OXcrs06fQ%2BztJMyvnZ3FwOcmY%2Bvzk3bl9v8kTFHgw5OsmDsqCf%2FZunQTpN8Pvbg6nPxs4OwYJR249FI2CnUkg5GoNBR4%2Bd8wXBExTlt5XPGqgsCIdUTaPwkzIrLEBjkpKTH4CZh5BghS%2FivN%2B83mFHRnsYIIlvoSNS25GkG8ORHvtw4l%2BFLVEn9fIlIhtZyv1mHKrh8zke1CxXSe0%2B%2FlbmJtwoASRDKkU0ZMws7KuGUW8MPu7QmRVVWApKf0imEgTRxMYZiWbtezkWMPxsumvGaBcBO%2BQlEIRh8wW1BBQba1yLZ3zq0LWcwsU2gtM%2BO15w5Q3ij%2B10S9AmR3jjEuHo4kmLNJW7r1C19mYay5%2FU6gOc%2BWKMf2qOAqbZuom8jf1FnHcPRTBE8a9KZZc3Kc1lnLTF6T8sh%2FtTzcRtc3M%2BDqAUSaPEbOY13a2saOfIgNVA71MI5LsmYl9MOZ8mm30GWN0PRk5Og5E3ddLwddxMIZzs8OHk2L%2FTmdO6KLc7wGBqT2ogKJ1VRUVM3XOwREV1JgwJ%2FwKZ6f4oXyiqKuAoce8ur9BRwyIKENsHod4Z83mEPlOTJ5wh7NBQaiRTrNlRluevBfuAFPQsYm0MVEifYgQlpphyvzAK2bbMI%2F%2F08gGOqUBoUVHejn144w4pQ2qqYMv1QsyTnNnhJm%2Fig2dQk9U778bf7u%2BnCafNi%2BOJNSr9TuPZz2OT6hCBoH4oPGhNLEe6246oHPhsD52kmdCf7kDU3hcQVTZF%2FCxIMflOwSDaUUugJU2hqtXFb3RWvg53skq4JOuViUj%2Fs9yTwhOH%2FA5142wuUrDTW918zYMw9fX0pphMcFj%2FNj2%2FmxOHI9kwWWMRuj1pkYK&X-Amz-Signature=ae56edd99dcac695884e3dc6bc500262ef8ee00a5119c8ffaa704419bb661bc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

