---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KPFY3BU%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIFljmd2neUbbP1Cz%2BbQjmE5tXNmFQ0KPxgjFS6aywKDMAiEAiIXm42kcwdORicZDsmVljb4HzEkAx0%2Br8PGg6Fu16PUqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKTC35i6QBSDUVBGryrcA9N083XoKXi33iJFbqs%2BIhRBMXRxM928FkA71UOt0ogjL%2F2HLOMcmd8x64%2FzidOXIoVUGp%2F4HSvvBcAjbgg6hm4yjvEPlJZUY94Sw9nMvkfNkvXIIHYcbsypISJHMjxXmsN2HIRQPSt3EDWjkSP8ymvBId4zVVwIdvyCrmgrJT1jy0oOqzsLTRW2cipx6APdiFuJHcwkYH7tho5CqgvAw2h1lhGlNSI503wdhQHBi8EGITWQaYghbRWbAnSWNYcPf6OncRQeHR9xHy8gUmHJASMypSaezlTjV15hWu7A4UOXIP%2B6O5gX2O733OzZmkUkakyahvXKnyWtYhcf1pp0oXftvVqZC%2BvfKyrL1y7jaJuk4VE1uZUN%2Bo%2BigDW%2Bq8In77oMLZnL4z1Ju6TEPl%2FQdvM1JGSNUWnfQ6zIpx94%2F%2FxIeuiZiYNpuZ%2FPssaYnpGdkEBuuT4%2B9ptZnBol2wUiJj8SiaIYvKrfINCOQcEtn3t24e%2FV2J8ceuVirfTxQUxxNQezVmuXeaU2RsRpN1p4%2FdGuNPYrfh8C9yHLwmxMSY28szRdPrBWp3QD2XdfXufyYSr%2Bysqkn%2Bt%2B4iaJmrDKGNoo%2By%2FHKjphEMpdFHqSDH%2FWrJOC%2F4IpFC2%2Fv%2BzFMK73nMcGOqUBmhN7w3phjIxIHHOAIMopSESkTpqRIJ6dqzQ8EA9b2NJzbUPPD85IRJ%2B1ueEtsc%2Frck0ikxMiIoz9ptC%2F1qWU%2F0WpXZLWJFKqCMLDEgq%2FhaF25ieWBFuI6xsjs2fJW7XG1Gwb9xs01F3uuhi5gOxE8JpvvQklMUoknyVSBx8kiHPjwTiEhKHtK6nrhdpAfe7S1UsQDcUhI%2B6MLlU%2BfCLjU%2Fbd4h%2F%2F&X-Amz-Signature=72818036b3421bbf41388d08f70903f5dbba4251185a4b7d741eff3a3e9e027e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

