---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCH3E7JM%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICM7lQX%2BMFSmCcH3rG9kZZnpc3dd8jKD3U9ZVBBpJv%2B%2BAiEA0Sh9jOo0fjGrPW%2FIkF3mFZFwFvCAeUFLrbR779dWIpsqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKWCv%2F1GG0OTszzYircA%2Byy1rBjqlH6meOkv6HOECpGbVRygBUWU%2BUpIfVn9WjQioMVRo2r0YabYfmO7ByUgKKEj7nH5q01u8bAIZzbu5wG1hJCzVGBzPPQI6bvnv4gfRAp2RjlQWZ55t%2FkrDsLB4Fw%2Bp5CMrUx98zOZ5MvYcnvpxCCT9k%2BUvisTVsH%2F5k0o7PNe0JicRRG%2FKOVWV%2F%2FRAAY4ldy6m88OqPN2HI85GtBBpziM1ahdYa5cJlpfC4om2ZmHNwHkWYFupk0W3ziQRye16GhAOkGUP43xuiUWuctyJt4phckYNFa3zCo7oFDt2JMQ88cK%2Bh1GJcO62zEgDYigDwOhicGxDDmcbdPSV7DB3YJ0K%2FmRNXWYbJ1lQ1qqTr4UX%2BdsJ%2F8mYgMKx5gjq25u4ABJZYij74RfMYy14a1q5rbSvHjKlcWEaUvRd5cJRL%2F2iwLEbayG2ZxMx5%2Fi1BeLyMpG8wminlHvblSLt19WV%2FDqTNBAr%2Bthr%2FN4i94rqpToL3hbaKi3EP1lYP0gx9O2g5n%2Bil65mIEAK0HCx9GxBKNQ1zNHy%2F0uoSJHsPNsO7oty4m2WyHUIKtGF%2ByJuol7BethW9ZoRai5oOzYSDihpxPnuc7WjWXF5X2NWp9vf58%2B5cKIwbxtuo8MJXKm8kGOqUBQP82l4M8URZ83ze3Hk5%2BBUDRINGv1muymzq6tDZ%2F6fCehtdWWxscAwuppTyGUBU4WWjdiz7mOrIkElYXeBqQmHKjNTc2fCW7nCkUeNZEUxLhB3c324EfpVwXZYiV%2Bm8IO6JLBycJUl%2Bu8rFtUV0J4aBTwMTS4yBnDsUvOIL0ZIGIgl4UFA3YNqujKm%2BjDcbAAO5ANppR3nwmFZFKYR3DAjcPYI81&X-Amz-Signature=63d398e2fc0ed4336cebbe82d18d65b86f5014d413ec762800d5ded8197540e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

