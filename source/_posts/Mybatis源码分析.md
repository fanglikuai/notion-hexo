---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKK4T7KD%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE7PBIQyzfQ4ok25InepgAgVEqFvWqmgHhMI2CmNUZieAiB1FLSzZpsmg9LvpubdeWLTVQ9EQatfbitlOJ%2FbiRh7OSqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6FQJnC22ooVOKUweKtwDz%2BHjMRuzOVJW%2BWNzbUr%2BZDoylTVIC0z91TJOUvxzOeo2VspEs4ZRljRauv1TBgTVnzbPEGhF6I2m58qm4GsKAwpXVnyyMuUkAG6Flh11AQoM196x3ln98fySFl0oCx%2BzQYkobEm5whmqMmdrkK62XrE0I9JrJ8Y5glf24UtpW9%2FeShvKNGpAsYjS5ZzqG61SZpP9M6sLM7UZ5oNGiPEaPcFnwexoVlqxgaZNXJfEG%2F3%2FNUjXnn1q6Z0WBts4JjdP9nvdsr8QxqlnhTqZ%2BpG6k5%2BC%2FkxDoE28De6MPY1X59DAeR3XrJaGGa8NAG%2BnnsbxlD96DQv1eebVsTT7ybdDZif6QLOxMTGGVKqIRLKY7qEeX4QZIiNvkD59TJVzeR5ImMFDcuyAiw2tUnFgdDrGuosdjOqebep%2BXMNFTSm43FGHkwZuiTHH9oHL9iNuDuuc72pd6KLHAsruqkb4GxcH0m%2Bby9f1B6l9Ct5yUdvvq0R8YrlMBdUrOTSqsxl4y5T9y6EqSeItIAzqrWfiSyHrwAddKqNsYHzGOS%2Bn9OGMYxJrlA0R7gU88rZe9uCxQfunGBTnE4wJq%2BZ%2FTe59KG8ZbLVNE%2BXtd5lBbQMuaRkZDrzKXAE51kSTp0V%2Fwbgw%2FIq9xgY6pgEmX4XhSLU36CXXy9LfMUPkrCfHUsnfO08zvhWCuIvzpDJnHZcmcCIPIbCN9TcululF9Kt%2B%2FnP4URgHgVh1WWMRqCP%2BWFQxhw6XRqF%2Ba80Ssc9PBvBceu%2F%2FJFGo99xogNvzpox5fwEttuFTh2rJcwCs5GO5B519I71nWZ%2B%2BHWGumU3K75B2A7uPQWTPbu2Qf8lXpGfOpxCoXsGANzRuYwgyuFN%2Ftn3Y&X-Amz-Signature=54b5577a3733c29122a7e6859492ba6862bfb76be638b73c40fd5159a25b59c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

