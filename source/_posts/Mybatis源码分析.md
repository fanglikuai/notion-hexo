---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T6ODTMZI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQC8Z1wkaPK2iT2N4o%2FyikoZXUUVl%2FnLDtnAlA32a2tqEAIhAN7snG6DrMnQIj%2Fa%2F%2ByQcnvMZi%2Bww1vvIQcq3RBTCDodKv8DCCEQABoMNjM3NDIzMTgzODA1Igwv6GQ3o7%2Bz%2FfSe8ocq3AP98Qk6iCUakxT1hp5SFbR7w%2B5zLnjMMaY4JCCxgjOl7b0J5vCfngHZ2Gs3iwe2kgzS1ebEBOpefkolp3Zmn5ndwDd8FqdL69LMRRyHqei6ZoJRXPqa0XjOV4MfDNjqcVW%2BgcvN9E%2BhMbW800yBTPQM2JoUtOoeMzNU65cS2yfSlWFy1SgUTfjL01e%2B1YPmX4%2FlHwRhVtH2IxnIxldqNd13TjvtMrNkC%2BfpBWUQ9dsiRi0TxE%2Bx%2FNKNEfgT30fA7fzabzduV0zpnXiiImot0vSvSS%2BlFOyE0uXqCXTHOJMDp6xoPqFxg9SpNdBApH6jyZtnehrMJsqwx18nBeysupbFm4ed4KyWsvRSq%2FFKE4zdn7pETi%2FtXGdT5VlxhHtQA%2BHQGAm7nFU3%2FdrYifICDOnxWTYTsbQCFe0scZtr2N1F%2FPer4mj%2FcoXn7uOdCUhuW%2BDhnGCZNOHlsQ4MlzAiC8Cw7TOMjp%2FTPpuv0NZIkLyZJxG9umIpjknhmF1cWLmmEOBpe3exM7kk%2Bc0R05vr94MQTZqYMJ2I2qGuU6wsH%2BR2d20qY3FVRBkQ1aZuhlm22mMxJqY1Ug8NfzRqtEiwqC%2B1jZkC%2BHG9j8hu5L1GfBYWykVOhaaNhSE2ESqeRjDbwM3IBjqkAa4MVnJUoq9V2toPfn6zbp0ezkQ%2FL2B%2BcXv0vH04CCsYuLtVKfjQIYqlo%2FmDx%2BiKxZSO3tIAiuBYRpk3GhtP4WWWbfxmvYqJmvC0M%2Fo3Euitbxbw6qC5YpRNXDIwWozL8%2BcfM%2FwCfpMdwxI1G%2FbNXflCZOxZDqiooEr5bAKRjdelTPZlX5R4eIzrqdPAb89PvBSYEB2NOvhZabJVqnrCxzo1LtyU&X-Amz-Signature=951e5250113c97fbafba43228415551bc7a608c679fa4ce352f875669925a545&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

