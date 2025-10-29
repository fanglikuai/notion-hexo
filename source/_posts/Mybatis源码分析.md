---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSU4GYTQ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDkIhgdK3gjYlbgG1RtQc%2FZrdrFV%2FUSrUMDV1Ifb%2FMyAwIgfMvrqOQXaTrU5ZTeB1Ge0s%2BTi14tg1KdGKc3n2Q3McYqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFSsoWlFgLrzxMcqUyrcAxGwBbbmMABVt3ixj8VRyLNEWlqLOtadMDogKWOggw5FhYuxtVud9gTFITJzRcvCsZJz8h4PLraigF9WrDPLAGknX13d6z59bDqx1J4AC91kiTiek%2BsSFLnazSQmtTSmpm9b9GJsKdHy9gvsPYY%2Fc2DEkkDFnRkJeBTRSo%2F%2B3Dx8z0b3eP0wz%2F7%2BM3sfCH%2FPvTMUdqY%2Fa9L7nu%2BN%2FoooAlpMp7QzSuG5Qy9bH1nkphFS1KBhjznKW%2B7txiLG0uSWKkhh3xvSWXbiikUEBZK0bM9MJTfox%2BxFKq0OyFYnk5TIIhr0x2MBDbdPMYdAZv1R0ehM3COH4dMDLsNo6Vqje%2Fcl1EcLhPx8W2Pro3BrWJHzkHgNalb6nWO8hZhsNDD0UDRRs1O1L%2FXLARpOaKBzoU0mlD%2B%2FE6mp6MYTSmWvjEPfjSc8lhqgNvMXvYdezy8ZTUHsye%2F1N%2FtRCxLRQjnweFNcuyPZLwF%2F0EiYUXgRLf4ulX34dRYmO%2FCAXoY9fh1woe5%2Ff692%2Btsr5D4Zt4XyYkZFGD%2Fd6Gb3nSkHlCU%2BotLAcWjUAc9TTWXf8KuWGVdzWTdxBwlhyIH3DCSX1HUaBsL6sBN8CKWUXFGALVZa4pFaFjG0gNxs7%2BB2Z%2FaaMJ2cicgGOqUBRTIuUajATqoI7amN7XoqJ7P4qSL1sNqSACB1879VWsNHKXWf%2BnBpL9hXlTo4ROqPcM%2BhPRIUBlivSzH6opw33CsqPWF2ewqDs2Ql6QcwIU9bAJL8B1HpXRUDPJdmYP8CVXzWZWr2b8Q5SVYe6kZ0V1n7Ts6OKCS67eLl9Rs%2FcrbQFBA%2BBBZ4dieo2558cUGxEXsGhx5diUA2WpubPKL2Uh8Uky6x&X-Amz-Signature=dabb8402626ac9beb71c6f5eb88b2fb7b4d9925f19b54e523a38760af9b392de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

