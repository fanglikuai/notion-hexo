---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY2U72HV%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJGMEQCIBvMUX99oFGNGO1kh63oOjebS5acpnYOmCmB9zQK3Tp2AiB9WuSWCwjzqGhhNGugEOFj7S3tQj5HzUevgNjYn3JcLyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjCaNayoq8zFsO7XmKtwD79PhFTZSvvtO%2B%2FuaAj7LhMhdcznJ2BsKR%2B%2Fmf5C7rlwM2KNqb7W3hbhC95wip2A8HmNcWlI6g%2BdZAiM9NHNjwl9yTxpXdFn9sDZaNY0g5PfkO2d5nVyzuMuDvDMHxEFOjXRl1zFDo7oox1c%2FSsSIORv49jBQeeRuJbH1T%2Bej%2BaCiieLLY89k4%2BMoK96KabNcuTpq%2B7I94jSQkocOa3Z3nvyNf1UspkhA44%2FAcUgld3govqKP730JVASfaT9ojewn3m35SyMKHU5Q17WtWXlYUkW%2FSWMHg8mBHcRVSVECNew1crfbSOu8WU7%2BFO2NdBijKP4SRXcCtJY5pROwX7kvmBQx8iR0KuDfzVaRkuAR0k6rBhH8xYD%2FOthqkp8O3ObvSuPVyJIXwmDb3rTqymLgSc3INzL%2FJuBfetFL%2FovDQ70yN641QW2Deh8AVvxdTgMdaAXlppsgF2%2Fx8AGgZDf59bnqSKENWtzMsuHgHdfkOtHuxDRV%2Fka1358T0UQJDCfXm7X0eEmDWvaCPpwOtRfm6rvhz13u9nMkdzclhMkfa0AQpPovYMUHEPrwf21Y8iNidX9jDjIf4NrlbH9YuGXH5waaiW2zlT3pcKw%2F9UEOP%2B51%2BLizT0ic6wErjpkwzKnKyAY6pgGDF9HFtfxjstH7tZnLYqkWF7fe9up9jgKxNu0Ov1WCxNct53Jirj03DUreryjr1KtIRGzdc2iVY5j6nPwHvSMhnPzWqXSQLeTrF8fEr64kYxuBFEprenH75Beu52H%2BXzlg2lC1JvYD7sjNNwVd%2Bie0G25AOv5xHNEswm8YU9YE8S0QSS2ZuqBj6kKFmqD%2FGCCcB74T2I5R%2FeWhQs7uqnUm3h1d5KDa&X-Amz-Signature=9f35705f4e79535289f6f913299b33abec1678972e68277df3b12db321327b8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

