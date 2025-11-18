---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2SJQ56K%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmXuT1bSKbQQRV01cHKHNMynluxothAb%2BwhNDaknGPUAiEAtpLdvuMh%2F7%2BLJ8qBrHoeOnX9Q%2BKYxetoSwtHVJct92MqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKfpeLcuGC1FsWgZzyrcA9qNcl9IcfVVHD%2BUXi2CoSfJpxAZssW%2FEHYNhpqHkQiALt99fdpov9VOPb19n5p%2FujIEiWVraTyDda8o0BVrfsLQ%2Fr3aaXeX1W8fGkN33I%2FUzAd%2B9qgBZtZ1zxm1xHOdyxm6I%2Fxe5P9dGgleXYMNKMIQyMA10%2F%2BoFWHC2xyIN1XfvEcacNHfYDkZB5Tzcv7gFdjrJUWbbVc%2FAUIPGKMTyaJtCxbzOdJXOCavN%2BbR9KbJHvThwypdkZ%2F5D3Og%2BV%2BGmO3YHGi6dC0ra3LTmUcBQYRr6UO%2BREw0K9RIHN7BdXkmwD8YZyKCx6qyYdoiTOpIfGSEnDOlo4Nm5g7zMs%2B2UihAfa6IvqELG8py%2BOFmaJX0yD9icCkXIIQLUzDenElz9fjgCq%2FW%2BLqY6h25yxWQyqgWGJPR8t7EIImgPgaMJdTd3xQLyq6aI4WwxhoDelUGORIOjDBBbFOHtDYmu1DgcP6Sdji%2FBT79fwNyHp02YqS1WH9%2BgAP6Kxt6yo6MItJ798kYEETpa890peoK6zMr1tH%2BZ4xGUTa48DdlRf0RJuVKqFxEexvEvjskfLKgTtv8zayI8UF14iPMxxurxzrBg1sZUXWFHyxfThhGoMSsmaWRXn4GHJqsD0MNCgxUMLyA78gGOqUB8SkpVITrSGW%2BQG3WyEmJusUJ2LcBwjowMJLjod5RscLIbyim%2BL%2BgVrIv7GM4H%2F5WHkk7M2sj2ahUp9xtPgqozvhuBkyp4D7%2FTDrfR2YOydTgKxJ8oBmSeBsoTXi25Wv2yPHzF4XvzvByVBfFnVijJ6d7vwTzGv0RySTXKav9KZHtvTRTymInqqi%2B7yFVwHPkUu8lrr%2BolCpj5tCM9vDyEZGE0Uwy&X-Amz-Signature=5b12bc1c411ff6399bd26edf70b4b20ea499ab6f1b6a4df16ad5a05655883940&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

