---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2GHS3UM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCyUu4juJ4lx4o3Il4SaoKatuAODWsvxKmNlGWbnMt6rwIgNg2z9Uy1WVs4YdXhyb9Ka9bYeJVpvJQtNuyV46YAKW4q%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDJjWS3vQF8poi3OCHCrcA1n%2BGsQiTBD3rfu1QXLysv%2BxicWb02f6T71sGjYFOz9WY7O2lhacsMHUzR323j%2BBVLCHeixyphaGQoQVUzjw97QGAd0rndrHBwP5mfapkBAmdO%2BlWR%2FOpUm4oI0ZjsnbElkplxNPwfEawkwLn%2B7Cl6%2F1t%2B4G3WsLJLbYZN8YtLNKhdkPVZTF76fKK%2FEjQVK8DNK2cm%2FDuj4Aq1RJv%2BcUuUVtyV1kQMz2DeGIRTIojbkOi1FGLgVrBethTH4PrRDbf8pArnwc0UoothFbZRKuLMUKKlztRhR0ATtV4Eh7oIAwIC3itiSnYlj3mHLCA5sW1Dtxdf%2BZ5750LdkNhPPprxxSkNGSncVysZo51YP12%2FM60eud4xcgMHT%2Fzs2V6e%2FOmoJKRi%2FEKDnvAGQjRwN5jFx%2Bp%2BAwFmY52KTkgJmh8v3%2Begq8HVJu4qw1ljhtEvdKAGgCmsqeTKXrJn1ljKLIl2FBs%2F%2FrBX%2BFkZyPId%2BQVLzH2CLwxUP%2FrcI0Nhlhm4IHti0IGqSWUEoMXpqfsp9DxqQjWbX0peKVY2JwlPlOgCIKk6nL9cpQHddOXe6LxprGSbLONbUdfx4m7ZiHmnbl8jDCm1yWhhxw%2FvJWC6ATbZbHqQYuyNPZthKvIFaUML3sgckGOqUB1ugbwhMc9DsS9VKdtUFoxUjedu%2Bh8UQIDW0eUT2a18r9F5xTV89ClIK91L9awLmMQAvkmQpaUfLgwaXls9HSZXEiSWes5jMqgu%2FNPdoRBEEvdOm1Vo%2Bj2P%2B81aSTO89VDrPScf9p2DjPWA4ztDVA305s%2Fod%2Fc6w0VvWw%2FDY9r33YggGv6LI6bbgXsK9GrEwyVgcr65L28HWPQxBYwEwdbi24rzKC&X-Amz-Signature=e47dae089003e37251a3bfdcca7f8370d09ab1fc2ef6ec56f834d742fa108a9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

