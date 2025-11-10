---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHW57GDP%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIDPqijqzeC%2FrBQj8JqqgoIVVBS7j%2FVQJyixBGWmRM90gAiBepO46pROF9RStlvn35dnempjyM4gexXFxvZRVDk3MsCr%2FAwgNEAAaDDYzNzQyMzE4MzgwNSIM1%2BiWHw5mYv4bLxmQKtwD66BFEfysSY12Xby2tuzDDva2pm%2B6BqLusabv05TMs330%2BkwV0FIW5yDsQaR%2BDNcO2lsIk24ilbgNDbON8%2FoQ%2FcDEVFLIMpgbotPAfbSQ3aQMHhvySQ9ksDvg6XgMJw7EnX0AvHxZhLlaIMehMk9lnbzxAI5%2Fc94zkd4tTIvBlgtjm%2BGjq1PLCmjnx80REreTc%2FbS%2BrQm0nHT8Tkojt%2FUfuLqdv4A4CmsyWsUAdk5qRcX8prL1mS58hyMufHh8YFSqeh5ntORSI3MnjvQh6XzaA6cGE3SHhVW98VuDy1pojvd2AdN9UbsqLP2Y7Da8jfpoPFJcCA8w1%2FXuhHe6lX4l9yMEXJFq8R9jictdmDNumsgzQtzMM1Xuxbo3uiCpiIoSa3N1nvUNfbbGCg7UJExkWXbBrB%2FDR%2BA0AEECUifzlriCZ7VjdGFwFdlCnA6raKPtssXP6tOfEVa8lr3igrhfST5pXTxLum1a5c5ztr4rouj96lMbCMtWYBOte5wBClqjpE5vBRIrGTDvic5xGoeHCKrENLQ23n3nptLvT6CHU8FqP3UYhfdkq2HF65MU9VPe0BE4cEDd731TGmiRtPbd0%2FHNhS8MxtrE3zy83zGn6bdf82Ow5024%2BeyUaswk4vJyAY6pgGAjiLDAd%2B%2F9fyz2ki0KZId5ssUwfuWcp97LQ2Bi7GtlU6l7OsW%2F9qv2BoQ2vTyLYIAX3E9glX3FHXOYmnogBDPM0YpnBOcCH%2BoecUJjSlVGbqBPjcPmE9bcXi5BYaA1zQpkC7fg%2FwNBJ9OcRUQ4TCid12BVqOsVMPQTT9Y75pyiRQ%2FvOdqqges8MocInmw2N7BYvwfCBf5pxhTp6A3FMctiLVNjb8E&X-Amz-Signature=d8b004e6251c9a80c7c8b253d731a959ac3c3e69dd0e08b1711232a30bcbe54a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

