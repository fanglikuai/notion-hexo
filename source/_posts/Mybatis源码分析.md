---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RHBEVX4%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQD%2FUxjuXU%2BA7SkEcct8n%2Fqnd0cPIZ1doOglF0%2FuThRlQgIgEtEg8N16Gv68CvWQYGlS1V8PUStghYclWgoZqG7QyrYqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA1zyadwdPKM3F7PnSrcA9pZkyu4X799CIWV1tMh4jxWnTG%2F02xqgUQwkQHtPXpv5vgzBl%2BvApUhxEcPhQmNLlFpcc%2B3OA%2FxuhrXeQdCa%2B78DYtMDQ9FlGMxBEEzQzfmdQGx60KNkayroyp%2FfTCiTQrk%2BCuCsAQjMJ%2BTEXB%2BNjroobAenJ5sskXOs6pGfXgPRVmG9%2FBjMNGuRbsBCGcp2vul57xXTlxjDtkHRc3QHDG%2Blcwnl9neV7glkbCbUwPK4eZFEd3vY31jaPDK4b64If5iHuSjZYmD1zOtZFDx9cs4rUOy7YNdcMviqneH1pTyxbNbGfhul%2FKF9WKy%2B5GUdNSZRnKTp8pOPlHNLDAd6drclFqiXEFJsSaf7Gt7Y6X%2B5YXfOxUB%2BgPoqQsby3CZWZ9H9EgnMPqpyztSYVWz4O9qGfCrcuLIYVILpEhf0dL5GJ5DUk848NO3zndYmk4JZvbqmARklOCwrT%2BXDnoXLxndsmo3z2TGYn%2FmcdEh%2BtnJ1a3FvemgkaNCmnCkk04l%2Fz2d%2F3N6MzLfPC%2F0Ji7PgH3%2FMIc082SKiMlq4pcILAdjhamkFy3zJmxJ%2BLeogc9HybCF5qzlrq9NWr0tg%2BgLcs%2BNJH2XrZnvyIrce40HJlRgrsGBMLXfXUZT8apfMMfE7cYGOqUBQSokbUFuQHXFfIkO4pbXDQYKSqJJonU0XJu0jXni4GxyCF6qvX1CJd3gaGa1xq4%2B%2BQCjpxlyiI%2FKwq2MH%2BQUrsMfPaXWfIfw0tGVJ4wJl01Dct0LsbokNhfkZYv1tnwslLB6FClfxxh807ttIuune2m%2FMC1YCgp2yQw7AZB7q16SKXhyT1vpIdOlRQXIO0p0zczB9uC65MFj%2FBS02OHmS5IMrsF2&X-Amz-Signature=052d7fccd7976d37b377168de88815cfa9e66aaf4ffec3f3458870d4011d5e55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

