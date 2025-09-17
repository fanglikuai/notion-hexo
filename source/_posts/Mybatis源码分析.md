---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665AQKFEX4%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIFgPD25r%2FO3Fp21qH%2FcsMx1N0SYQOaAIHwVx5JBEMyrNAiEA84o0ZeLhEW%2BYDpMekceXCkWfZgNv1fuGZIq9szyTkesqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMRzFvTKZS33YvoILyrcAwKP5lpYtd33ffZJsGCMY9njg8kK9FVDlmMFVMWezJVlS6zrYve9bYJQmH6RlkuXgjiLKXI00ad2VE3oTYpUMJFwvkBRYiwiALnKxX%2F0qs3FEw%2Fe2QbtvLs%2Fjnvr9OygDzsqCRabYzPX4OMEGtaV9s6itZsksqRYxBLhyGAd0EZ7Ijjnky2khRX4E6zNNe%2BjTWbNI5ovCdqJAPms%2FeaYRCa5JfohG8O2fDadDsB5gHwbKTqia1H%2BuAQRzyHrx8ZMg0U9CEuKhI0VyCYwYDObhpD2JapLLjbHQYOxpfOrq7%2BXm1Py1KVz3QkjkVzTAL91m%2FpE3vIW%2FmSC7cgP9zNuKSk3aanIWBaMvGmweJkSauHhfahNLo0%2FkwoNLs0UPrLz5T0ZR5un0T4O8bkXnkRalRv0y92lgftBR6dkJ3ePts%2B6hhYf9MRnKCjmfvXj5xDlYA19mAMlZyx48yFJqLCV%2BUkUnCfw%2FGFP90o30nGJwdWLloyaL1snOHlD%2FjDd0DRnSs9Q0aJGh8N9XMGvQ0Jn3BQ5uiyAFlYg8NZtgAQFggMJPLXsOexiwExmvQ4nAkem%2BxS3KmCuZU8tsqZFLx2rXFmvWpvTEcwkdyfWeY7a%2F%2F0ZnArKGabzx1bWFn74MPLbp8YGOqUBR4MrzON4wwuDDQ4KGXUSIKPpIcSHDqLi%2FNrEpV4nNdkCzKFc%2BtldySJZb3qqXsKqXvaEW6s%2Ba5HeJN1rqg77AifOmXfJfyCbowCbOqckpDVd%2B97Pnz%2FLHDP5CRAxZxy4rSwEvd5cebiqyZRaGk63xR2JQSzPIO74dOHC%2BLGVLVpcYFvsO%2B8manfePSn0hveObGquOFnO61FoDR4upNHRwpwweSWT&X-Amz-Signature=081aacbcfc6249e859667d073f7297122a7fde49c5fed3e1c047d24bfa035536&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

