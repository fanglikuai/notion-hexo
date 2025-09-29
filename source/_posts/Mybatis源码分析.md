---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WD5GFKYQ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIApZxBeL%2BCmAhPSaSL2DdCGNUWTey7HWK32LuJapa2b9AiBjbrlspTzqNZT7Xyo0NfvnESPtZ68HfH%2BQBpzQsTu5EyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0IMDIEu%2FN2GjFvfRKtwDK9OY24344QwKbVpxlkfLc0VoRcqiBbOJ74jeaPcYqfVGolcep89NiseVb%2F7rrr44Fp%2FQi3KG2LBrTlA8u9dknDzdfdyfTxQuZglZpP002lozKjX7QqLVSUnqhht9MmJP%2Bmhz4M5YgPtQXzE%2FmxVuaoXr%2FyUj5PzeyCz%2BkJWpvk5s0p3M5bx9sr0PAISHkfjQmBFgvUSB3HzV1P62dhw9uvmMptDpyRVrd0d%2F%2Bved5yxIFGLL2eVlqif9dCHhtq0mJEfBX2bbQtIAsoVSc2qLqwDJuN8Jq63RN%2BcwlYvONSrrX70aJl7Wc9xqgnWncDLOviDwwRY2A%2Fsr%2BCvKAtpAmV5ik%2B9F9OdFwnQivXOuPZDAokhRvU0%2F%2B8TujDYPVcxoO9LXsA%2BuTTO3%2FKT0wQfLdNmpHqsowkzb4hlGVk%2Fq7nKsHipvmKTverUJBwViRf8iG9c6ajTWUeOA1NZ%2BPOozeiVzZKJd1heWKuLeGFf45G4ABhWm8pNHsZycuLHimmlm%2FNR%2F%2BitLaImWxavP5q1s02PCEneI2xgtJg3usaQD%2BCQfXov5nPGR40tXrko%2F%2FAV9BNnJz3MwyycfUK19i%2BmvbwH%2Fl4Cmd%2F%2F7XeyoTq65Sfjfj6D9ky8BoK6NYVMw2qvnxgY6pgGeY9MZowIVvSxgZ7HmDLnGETXvgZF3GwwDrGZWNNMkEbeW1xefL9ueMQXkkZHkJtg0CJoS8qKLzqihH%2BOFr6I1tEEUBM65uSyk1eYvqwmFneRBdnpTVQfY2hce2ynewGCCCUEtzSaRhHz23nGn8rhBnRHZHgmz8a0%2BeyNsk0WD72VGmVDrtLy%2Blj8JY5WpwYrEqth3LCTpWVLO2eA%2FkY5unmAiUog5&X-Amz-Signature=24d4bb10a0a68fa17414dff6d345b8afbb0afa5b4875a237b3f69b26344b0b14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

