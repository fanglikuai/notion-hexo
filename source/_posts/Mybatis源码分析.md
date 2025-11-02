---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCAG6TD%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBQhJTuhuqDmw25kk5CmrqfrBxIqLNMxVA7a5ktIU72BAiEAhBT6UJDF0eLSLWbz1wSMgIAPoh%2BBAIkYsHDV2jfmnp8q%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDAygsP4%2FkpCGvWPplircAyk3s5pKtVg3f4IgXKAJMVWEcrURv%2B%2FyT%2Bp6WZIxkycwGPRrnYUu%2Bme4L6vDcGWgOMOkbxmMbRisfeZK4g9W37ScD%2BBhdnvO8CJrHx3RdFQQ6eHXySPHtmBd5tFnkUAn0c4%2Bk%2BDJlgx6RNH9gTdypU%2FNHDFwq4mQexvaZoyCeY1d7ii0Hp2M2S5zhM1J2jIxE0wWy8vdEDBvhVoXJuKWWSjwcyXVC4xMGWRYpneKhQO1SepsOBBeVaM%2FCEEcZpvBZBQ2ChO7pHFOtpkETFUc0BGdNJTSGk7y0aJQI5bWp4LGEpkRKwL%2FSfM0ZP5XAZ0iXDucu4PnyBuw6vNkGtCw6RrA9Zh4nFJFL2fmaWwLxzd5P1AYnWodPLoNPO6BsRQ1VRqJWDiTVgh7SHaO6LsbY3AieVP228EFEdenRtgLjJpzyyI8rlK9q9l3mH43rjy1jP0AfBHSdHPHXZ1oIMKmjk3w232ZGZu1XLYPMj6d%2F90kJBvV1VrXeJ%2BsKYu01kolrwjtJ9iEiyT01Ch9XcCPeoDHG9mWUl6LKQ%2Bc0AKnClUU%2Bhtq6%2B7og2gS%2FTwgBswu92h5aNl%2B1RSDNOQhaRJ%2BnhtUKq%2F5dLtjaevzOTDvA7azsEJ047zkgnXmufC0MN2DnsgGOqUB7u%2FYkGzJJ0RBjeO%2B86PvP2808SJS5GZy23HMmL0hMY0gtVXF%2BIL%2BwKbzA0prG2ysNAda5MMiiFEKWuAXO0gZ44%2Be7v36yOgF81uTgfJ9TKtsi4om3nqJssre4zQ00O8DgQAD8gxJ%2FaEHlkgC6W3AG3E%2B9%2BE%2B71uypUlTgr2MkprMQs%2BhKIklPrNGnWDycckDhz5mdFsukAZfo5yHVBMn7bj3M9vi&X-Amz-Signature=0152b53514c77098b83ff7587e0afd5bb9afda66cec02dcace955f761331288b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

