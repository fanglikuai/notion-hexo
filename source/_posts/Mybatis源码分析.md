---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3ZTOAG2%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIHC2y1X11gp46P0%2BiS0mxgqHLthyi5mjMq%2FpyEqjxW6wAiEAolhALmpbJEoC8R8nXUTa7wD7HaW6MBwuwidHjEcItZ4q%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDFxPqo7zYUliNUcSgyrcA0ae0VCbNuSABK4J%2FbPXY0anGmWKS%2FogGoJeahoFKP6mjM%2Fr46mdsQJD7gd3kah4UwyhGasoY33dQOpkYqLzttnBv1dYTolHuvhk%2Bnj1IbZXkwOrgSwAtS1ixnHKlDv1FgJWu%2FV%2B%2FS95y%2FvWI5wz8EpAF6SkikDWZxV7GvX5beDP7Mt4fsbCspGK5dGBf%2FoPTip0UzGzkv0vmlFQtrD6%2BOZeCIKU3jUqnGswPNoP57Vxx1e00D%2BF9CVSEZwBM7CPCPMIW5uVq80i1KCrSBQTVZfmxdJcRjinM9AGOXIKJxtZWLRK7Diq71oXfx9aVdtPcazE0RrYSi6rBKIrk37wovw7%2FtQ%2FtDUKuvERHSnhAsXg2TGJKmBud37gU%2FMpx%2B2apVMHW4MvjNwhvQC%2Ftg95zXuxCVLqUIXrjNLcPf7W5fDsSW7ocjMAdBWpg%2BaA5uZyrXzOrhG9t0gz%2B%2F6DsTt16QS0oMwjlykXGEbaO1epW0WAbrZgC%2FKRPiqCqKTGpJa%2FBMjWg0YqZQ9jpuR83ewTVYNxI%2BBdIklQph9tCvHfhK%2Bp%2FCqAZ8wpDxwOm5TdaBtwb2GAc5EYdRscwvA%2FCPVplDkPuQ0rLvNvMA9KrZjKKLyJW7ODvvz2afhpdoMvMKCJjskGOqUBbd9pBzkYoaXWnk%2BRZyI3Di%2FctG9kT3T5%2BAB%2FgffSSx9QNlNum36CbKDW4GPwaARIwNFc9CdWb6XgASljo4bUoaKdYF2EuTl4UwfiZUL2BRRuh7DtZBaiO6xKT63wSWwjPa23DWIfmRIuxD6oK1WqhbZZiHCxgnYZaAhW8nGRSq9rWTDUQBYb2sxZ%2FPT%2FpUgbQRQN%2BABVgqIcED4Q8DaLSMaFgIU1&X-Amz-Signature=a9df836af2f3609c7e93ce67ced7a829b00a440771ba75756bc30047d89ce499&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

