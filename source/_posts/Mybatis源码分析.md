---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQO4QHSK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIFnQb8oLnxhlxvkPXog7umtzy72MSTB0GWuBFcwXF36SAiEA5weTpRpvwDVl9PlU091%2FCLGjYvTD85ZrzS0jdhIk6XkqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIuQR78HjJd5FUcZbircA2EGCoPC9Z1%2BZ9JOojymoVzA%2Bmg6sjTJPR71w1miBUjACiTv3KYMaaN%2FpQM5HXeFLC8WSqui7Yt2P1VHHfBSBjONGboAmT5aWgOcpAw8oqg21GSutEKihcCzLdy0G3QQZD8fpsPQecPT2hZrizpkN0MsiGLrq22MuhJt0ifYDQGZCMikaBoz%2FR4tHye6JpzzMhvRKZCd34ASmWEbeKWImkPpQWsANkDPMeEXy%2Bll2%2Fqod8NhQKcXdJ3USSr2ZdcAo%2FpjvYnNDUJJZkEMtgYq2%2BSIV2XAOTnQUDgYreeY5riJqbvvq%2B%2Fa%2BKrKzYadZrkcR%2FNytRjt5wSTEfenbMRi7jEYToWS4ngRDif8gfRdsKt9hgBP0cwaJQuyXbbRB2gcS5vj%2BAsiiiCp8Ko6FKwwqldhNxR7DzuIlMEv24w2bPnCGgVjkT%2B%2BWyJ7%2FLaNJkT7Fn7n0ce8cK%2BA5%2Bc9NFgD%2FRFBTm5gfCJv2bp6cWPregP0OvbVyaohbAr80ZsgELwEHEKBP%2BzwVSjIX17vtEUWH%2BTcxJKi6V3Jy%2FFL8YSKTAnM%2FFXH3xx8PbUT1d2Rjx1ABSsmtyQdaOIsAycgTJw10HKI%2B8sMbAiJtPdf4JDkTgqEwivVd67vi4tuSSeUMP3Xq8YGOqUB26x1%2FUU4SPP%2BjHqmmbYyUjlFa%2BKb5tkT1VOnoJDBoL4zRDS1gNwGYgzCJKbF1IRvwH6mxo02usNplr9xEs1zdhK7SRxSVeg4imaGLNMOmDvrkThpSCNV6MTBryb36MmH851bA1fmjU%2BWw3rDEIqEPw0bL8iQ1oFiuoCeVhfNoOJoIwZPxn0dYycj43fSP8mMS6m99bfvbqWv8sllM%2FB9zLIdHHjG&X-Amz-Signature=517044cc32ca012378322c307091eb1cb092b23fa7ec83696f02b9305b070d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

