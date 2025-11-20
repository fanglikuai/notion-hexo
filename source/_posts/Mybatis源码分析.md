---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WQVVCHI%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIDUul6by0v3AQKSzJfzU83HVWx%2FiYPfN4MXorDtTTIAAAiEA8dOXgCFyjpn0Sjxr3Pdd6A%2FHsp4c1yIVE9y9kDMlhZAqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ3%2BnzUUsdPeiKtttCrcA7fufBYN9NR6nuuqwIeWkP%2FfJp%2FRlWulqcpq8F5fnmycvJ%2BnHd3UbHW5%2F%2F8OjrazAkBeSzzuc4s%2By0fijhqOrPqBVoNOcmW%2BxhVOBwKiUSZC8J1xhhuqBYwxfA4g%2BPAnnyi2h5tz35hBzZNhYlcgaDQpjGxGOpDOdnMFJOGaiL78265uCfmwSSZdIzEnoAWVFP6ZNRiQCtfMJQjWPl1g1DOxyeDH4XGVBjXiB8Z05XDkLHclJcFia4liydAc8W18T1nxZcrDw%2B3O5lHoPI38YirvJqpxLZijI4gadEthGe2qBcfKi%2BwoEHgxV9xtF%2F3qe3OpWPgCfTb2Hx0FPR08ijPRV%2BQj8whdvi7mJGjoU2nBI8bkNhcdMYHmpz1nnuFG2hChoOcHU208%2BI1j0eiL%2FtEM4IeOVyu%2F5qg%2FlWAQ3zNGuFDB3RdwzKXr2t9T8gfggXR%2FJq%2FxKepBlRr0WXxxolkGCl4exSQjeK9o%2BaCa%2BrGB6G5%2FbKury5dTeLIfsSM3d5dCSyvd8eM9foh4q1NRJp%2BxgDVlJ5Cddsp2SUxIyP8biGtGG0oS1%2BTsA1G%2BgQQq2yUP6Ea%2B0OmJVRs11%2FIRStcNzMnh0Vx%2FF3vTqaNX1xybZFM3P1nrCrYDVIXYMJ32%2BsgGOqUBuzp%2BtnVLWX1Vy4skyljww2L7KXWivYyrRGl%2F1xY8sWm%2FYaBRtBozxTEWlCsvSB6dmv%2Bd87ws3TuZhERnSDZetp2xHBP70VvA13zK0z5Oj2e9ORilKOccnDzt9XpRNzD8N5mqN1dX%2FYLQGv7JDuJz%2FkbNwQaHYmGRprqicjpoxmGxCzuzMQ1E4jmiRLhKeTSarm8TZRLDmU3ANQp1MaxVD%2FYGFevs&X-Amz-Signature=5803c13e36ba1f501fe0b8c976ba089bee04d08d8d4222b530e64480f00b6e49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

