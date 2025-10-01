---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWKVEDRS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIEa1frbP9qRVmeMYYfAicxaS84W8sEPkrGumLz7NDKy8AiEAz%2FTj7xbPeaFLAeklE8QLLYLldSFp%2BTGfi33%2BYz81tjsq%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDAqPbqA21fc1AL90iSrcA85kr3J4P5ncC4AH%2BBMHeQ2qRkktgKY9onmWt5O150tV7u6rpI4QhSOTw0W1YIwllaQesRB4xjB7lJGpFs988v37gthlmkT98Yrus4%2B0sCcs1WNiLyr7U27GQW9h%2Bz9%2FZjQwnntkUhRxMsNMHHeNAUEVfyD85IzsrQUGXZS3aE9vWYNhDNCGL5i8ry2bq54dY8GiFednmM1lPaAGFVDue2w9KQqYMLjC8vFlOICbtRbIvbBD5vHIDA7ZLCNyJ7y01sDBTr6%2B82CQpGN9BClQcMpEnXtzB5buWHyS8KG4U6LxE1bYiqK7%2F98H5wsphVyKgHWHog%2BOMWTaPbCyf61V72lTFvhvjVBAgF%2BYa5MoFUS5%2BE5fbZUvNGOnlDhqJlLRHaKGTGay4EOdRXMGaoAdpv9NLVuHY9DEAFrtc9miPfMp51jR8lLed2MlzwrMYzTFPOHKqsFsfFuCdGgV61Y4cD6MLs3vjWnv2kJKGJqV9Kt9PrMIBe2DwW7Bh2xhVyDScIPYwrNQ7i%2FcQb1thtt3EriorjGqHto6oXa83xhEAVyCg8B%2Fk6ic9FvXNuzNDZZ6X6WKlPKi8Ene22yPsf5YSmnPp9JIrNgkUy79FBLbIZNihtoQYkDYiInVltgqMI2c9MYGOqUBSB4EGXDWDPmZBvkMjKqoyzF4CByQr6aUz2aeqsPCI4S5MM01mQ%2BhYC2XM8VUBnBsL6hKGwdRCHy%2FczR%2BS%2Fk6ZpzHDo9pldv0y2AVOalBBkNN4QRBIdQDHLdrbG0L4ypeioJNYc%2BDMe6lNOXYbobFOe79UL2Edzi%2Bmikk2JhBSQ30VQoEKuY9jCTxMMZVJDUTkIqtKp%2BFzzT%2BntRx8msJyQcJ2ENm&X-Amz-Signature=8cba01c042da72b7ee84fa3de72639d59e8ade0ace897b9c202c6772865b1068&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

