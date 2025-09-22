---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V26M7WT2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHJ483PcWB%2Fid36DtKMxMe9KDyYxyMDl4xW%2FSb2kEx6AAiEA5daK78KpSHg7I0UfCIN8TJaeGCPDRYZJtQj1%2BGGeHvoq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDMr3V%2F5Rnu7luFOvPircA71M%2FYmDS1cpybDR8LWhrITeorjSE%2FR8w2wgbKS974mp5Exl8Q4fhP6dpr9QIsxPUqF5CWrqQ2v2kuT%2B2ZGFi1fSKT2MshzvJFFTabtp1vl6Y3FOZ9xii3sbWaU5IO%2FPL2I72t7WshW35vIIML5TCrOP1BgEkDejNaM9w3T6V7nmSaK9OPfitSzHOnUL%2Bf8tineuTuCfEruyiNHGUlKihIiQhKvsZX8EJdmWENyY1Bks2gav0XuHSlHJPM8MZJZZ9iAd8U0BBKuLm4525NX4OOzN4IAj4khJyEZfHQPMfV8u5XRhIiy2xx42bOOV5NWESBsq2twTTCBTvo4s8uNwlFW2SSUKCpk%2BVal5l2p9wJ1TbGKTvEneddSAIQuOm%2Fnqn3R7PSpU3mwFL41b%2B149dfcwWsNlVs0ygp4wrLXVD8D2rMLaVYRN6wtbdIZ7s7hPAhtHjcYfe8gKlQZaurIbygw%2FxaVJLqLpNO87vp1DyOF5TDO%2FQUKiItcFdQiE6OHYBblto6mNP2IcSllPPsFPl1zrb1X%2FGo0TURbRHE5ZnVQCIkgjgTbslky2h3cm2xni6IdywMpyyprkZbRMYsteYtlzKipcG4CXvLWvMW3HemABiupNU00Sz7DtdPv5MMygxsYGOqUB1DEwi%2FX4hZd4ObazRvKezYfmTN0ixM22ZrLYvr%2F5OhjwwZkfdyNJjID0N%2BOzbFFOyODlmuFFCxMKFyZxhDv6eFZ7a4Nw3K4RgGGeIIfY%2Bjhl7HIFIt5ny3Op6%2B%2BuT%2Fr84DAdBDHXNntIm9rSREwXwXlv1Kt7yCwZXLgtGCvBAa%2FZzNGXmVJZ5gb041mBcUNeDdIr1iaS3a8BsLf5WjKYA65WlnMV&X-Amz-Signature=6679bd9ef7098abc2df85632cf3a090c05fee34ff72a8e9514985ae3f893699a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

