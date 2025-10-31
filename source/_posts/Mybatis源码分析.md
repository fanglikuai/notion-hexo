---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LPKP2FP%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQDkyZWzrfseW2zOxlAXs%2BH0%2FGIzmElii%2B01%2F5QsJQmBBQIgbNv4F%2Fc49ZEtTuxTfLZ%2FLFukSlaPUuXHNtRZNA0hM64q%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDEiV46mRnn4gQnXsmSrcAzRJAMEN4cBNYlI8afDqAxTsbzhKLe1D8gc25wNmVLtFn4N%2FZ52ZXTX05n6w12uGakTItl4Bj0SH1tMIBpTaywhrOAcB%2BTay6MgnsckQZ5y0lHi3E0pXkZuc3WVFxSneWW57BdLu6zDf3iCAvTv2mT73XKKEDJiiEzitkfekdWdzX8UjYHtsi7DoBrDoD3eYeHp2NsD7%2BRzL1va0L2DMlIu3XjZB1MAebDgPjrRDcDfFYFVTr8EDUO83RNmm3hVlV%2FDy8E1kIQZX7h1whAgK3MDMdm3jLkG7J1GBGrdxzl%2Bymiyt%2BDPqm6sLSSTwdhecoUsDcB6KPKKsHRrSRmlZeAb5EvFxK3%2BHOaNxj8NRsvPMIDr%2BjTpHFtFdeFsnoTfFLW4W4fI27QezT%2B8Wvle%2BhBsjCqy5LCiv9yCNpfUMWvUZz60Te9SGdvAzo9sIKYgtAehRkiHjD9urrYO4ssXIHzUwVSWVItzYpTD7JtScPALbVXe72X1MLiXjxMltHHcRkDwhktfQyHsdOnjbPmINuAwrxa4NTKH1faa68d28mPbt%2Bdt3QiRQxcdPyCANJGlIRZrZzUK%2FOJoYpyBLlVGf80AzOTkW4S6nf3OXGz21RBQCzFjNAGRCX2lSmC6yMMjqkcgGOqUBRNKMRuPIoRwockExnP4vI0%2FbYA0eimRytj9Ylu9Ae6%2FJ3fqh1rO40xlzebnnqg1KHquthh%2F3GSxXa%2Fi3jqo9%2B29iBoLUPHSkMDFtQtLiSrPa%2B8FbKec9UYu1NVHSLwGW5TCo4hs3adpoRr%2FavBVIAdFScc2MyzYymVn8onzpjhM42QVUILxVTX%2FGlTxuKm0c%2BWbW7ePuURqLIR%2FOmf%2Bog8YyJjOr&X-Amz-Signature=bdee6d8771c74692d56434f3bc9d2705e8bc5e0ac099b2d103c42aa2707fa98a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

