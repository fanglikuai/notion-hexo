---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EFWF4PY%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIGzSS58ujsxeORYUTswkEctP8ulAI18k%2BbGoeXwjZzvqAiEA87OEnDCGmrljauGRNUe9a%2F7ihBvUh%2B9ILBZSXvypHTgqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIxbel9EYyBmPCQhCyrcA1lDtPlRtcg5MDfd7zHuczUNGMifcG3Wt27TVOMg0tseNmaPKLmfAs74%2BepB%2F4EJb0R66es1hdCxm%2B6Lg4bAr3YKxs%2B9zPrlMY3v2m8fjUB2TpraTWq%2Fx5PGqlDOYSkRr94Tf00pBw4yadT%2BfAvutp4XmSGYPJ%2Bwd0OeQEZyTAtZf7Hf42e0BoNeKPSdXalrT3uJqcdH%2FM13ztGTbdxibKPwc8S6fMdTHEhq2o4FN8yJIUxLWMMlq3tdRxi7CP0y%2F0ggPphcjhqMQBotPglE31CeaiAE8YSbu7AAPqeXdhbypyVhQDojuaCMg9nyd%2BKm4lbdDrwjOTJSePMowRkT41Ke%2Fx3yaXQMgr%2FWKSf7zeHzkiPJfIWYcQHX08anEBaqMnRb5pGkqyFrEJtD9K9k1X1aU9WyeEtjYfoprnRWy1Z7uOzYdk6KUA4Ev7NodvBVWd4wAT1%2B9JgUw20snLOk8LEp8fz8tJ7OOnq3BZqwtHz2mxuB%2B7ygUcvD9crWPl0eu1rROJmPGusbkZ0N9dj5HkeS%2BR5hW8%2Bnahr%2FL2BfOKd0yAPakvRYwB5I25Kh3aIhAeFXyIVoIKCYmr%2BX0synEATNfBw3cNkISwmSpYXYbYf0B0ZATSTDoxp01Q%2BDMJ%2FOt8YGOqUB36Jy27qu0xJt5YALq%2BmJZLIhMqP%2FdhuZF362DaCeC%2Ff5X3EXj8%2B8yw54OPsg4S%2BeAuTCbsOb355N1QY0dWLX5DTZh8zMGIfTri7xGrwbws%2B%2BaF%2FNVHWxkvfjFFkSL1RRgxlHiy%2B0sbRwTEgbLbRgXOOGlppe7LwtAcsOSQ%2BCwXzGZO19Gp493PWiKQf1TeMOfLb%2FpxH1AaK8nzPdem2UZfJqtrs3&X-Amz-Signature=77e5eecc4c96c4bd50ca0bdfbcb67bb9df293e25140cbc6409f2b8df069f5efb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

