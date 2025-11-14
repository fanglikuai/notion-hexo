---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635ZPRZM2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTlIljWGm82KvvYZu3XVPf%2FOkfpSBbGa9YEOIrGvn%2BxwIhAK9oWhxzvZXpR%2FVC2ZeuS%2Bqj%2FgrzmBIzu%2FG6pIqBAZ30Kv8DCGsQABoMNjM3NDIzMTgzODA1IgyMlTA3Dpz9c2Sz8FYq3AM%2BwJPRqiy%2F4pfQF1EHqy0eoSi8VcRan5gmcPGVwYPcDoE%2FGejf%2BqhFeJF3wM5g3DGUvr8ZojDYSpFlMvyeqN8EkiMFV6okkvUgOUdpJdi0C3T9sWTBoKeDERnlRM55xnMxrD%2BrurnzTfZ1sPaIZnoCicPnx6A9FKH8X2W3BLt%2FNr82gtDrJGyfV7l8rK%2Bmy9VLPFowf78iM8n45PucQbdzGeI7%2FYoAYRer0AOd%2Fv%2FY8ksKrPHoG5PNZc6txqc6dX%2FU92iYst1WN5EI0lGYWENR%2BWrbTtun1TDYVdEpfyj7DEs93bpVpwqN7%2B3NYyVEXOEXM3MBnUgreoEHh9e%2FT8uGVuk4zXlsDg%2FQ4luWnkjCx06eWVaX9z%2BPNrU8r1SoKGqhhWAX8zC9L0x1hLSzj9%2BTTcVi22TqjcLCj4NlSAlOB6066dzi7kgiYSvm9idiR%2B3UJdZtBPGsQkENaetorMGSXhJVtOXeAAV%2F%2B6wgfFKGzSQ6Ee1e4WHkqF7JlFbgX6qtdFLzmPOpn0uZ9awCPbf6xZexrxje3ovR5o1J0PMZoxHpDV2FbR82PP1ms%2F573akk%2ByC%2FdBkRVx8bLVcBntvh%2BMLBZHmI0%2FG4SeangHX6TwN0ZaOM660qmfK0UzCS0t3IBjqkATHn68RToIySPMgrjfHwVBltM2qRch1O%2FS85UUi4PG149nCrcHDr6c2EWLcLfVbhnMyQ4EeLd%2BQjm5evxWVJ%2Bz8BS8oEjFxTNhInzJcYJVqkPOCcUG2mzcZiNHahLXZGfLoeYhaPwRpGhidnDFgw71u6Q4Ed4N6tZnz7NKjeBrJO49GhRo4boIP18ExI8YDT74NV2Xd0y9CbhsF%2BTtKxOmgLGafh&X-Amz-Signature=729803f2832df946f6fd9b55293a33203951cef1f9303b074eff0fe2e9f2bbe0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

