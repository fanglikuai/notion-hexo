---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YOZXYIU7%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIGob7UOxNnKrb4HfHlaSooaYN50QWqg8fHFzpyySRdUTAiEAnvZQxOZnrjWfK%2BMB7m2%2BC173YEk7uHw0M2Jqv26n36Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDAWZObrWXap5AhKnJircA6W1O7DFpEDGDXIvXpdv%2BTd%2FLIU3GAptNmXzQRqzASxRM3smByOMkbZVOJ%2BZdvC0SY7HxY5MTVt6c6JoVLVPxYRpR1nQcY9P8XeZwt8qB5fDZDXrAPDDCMpMQbElL%2BnDmgIuLqFpoRcUf6USymDpzAZUKqMFXhzIRbVFrKdUJhGln%2FZXY0jD%2FyWz5YE6qqWJC6I1s230EW6dzPSNnArkdIVz3IhxS6HneXyw0fqwTJsoF6P%2FUO0qP8smX8HOkvHvY5er2qheLQsa%2FyUdTEq%2FsVUv5xgJ%2B4ReggeKGOenfhkmxjk379QXtaQV1UJCltXyOw%2FxKOl2%2FP%2Bnw3rDFAAp62%2BNwqLr0DiPNdjBZ%2BBbYsv82RQRIyaAOUMEErId9BzLqyswuTVbVKNPB1PbQH%2Bfr0i76BdIJdNBHcSqi5gO5%2BoVkVaM773EGVgbD9Gnz%2FifuBPGgaKAr3Azx5lGsv2QTKzWkiPXCU8QrVLiMZHjII%2BygTfsgkad5Sh%2BIDsrJY%2FhfO7SFO9l5pZramtow5GvGXI0GuXtUIDPrZmRkByGKflg4ZddO1xnitHwEQCCbvGqRa1WVQxickHQRvcCJ4V%2BkcbDgdJnVmvS8c1jI4vs2wKW2zfHV%2F4xi7BeBZDdMOqLlsgGOqUBXvu6dVcR%2FTU2CSq%2Bsj3itPSS85bjnaW80hMRbwH%2BUuHWWU3MB7P%2FuEcMVUbrmvKesjcQxrMc3qP7uk7J43%2B5%2BBqJSMm520xh64qfhKbck1aG6C4n%2BCFTlSyguGeNrif1vNoma3u%2Ff8IHGMRCahDJrT6l9cO2UGWK2in5QR4qyUMEkijzt3tStiNQOvd4A69YGL6VxQsyslbupbkg35YAcUmXK1DN&X-Amz-Signature=34d84a25ccf99b9d9c8da849408ae3463e788cb7fc1c53d960b6594012026997&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

