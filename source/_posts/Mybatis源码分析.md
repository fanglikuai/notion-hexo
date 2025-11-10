---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHIKXC6K%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHesIzcOt9Bxan09VcinH2Wna6w8%2Bkt4QHjpmKmo%2FFvAIgK5bdi3xrk9jsXLCUnQp5Vf4sB7Z7QsRDHm2WJaZk%2FAQq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDK4evqEvi1mzF9XvkCrcA%2BkrWG1XvTTBMdtymcaWPVnjCHgxqPtrH1nDuxzfojZnPbYO240eTTweHv8jereJNTYP6y6K668aiQkQ7fe6pj1lXWbfGs2GimnzuaEc5Ij2QNvzbhbjiXkE8d2gIhTJ6KC2N5mYGznRGkkB935skhsHgHBuf8SwRfWNr%2F%2F3gMzMYD96ozDViigpytsJkHNjINVc9PousgC69Uj7mkyCrP9EJNSypT8yTLVuozrxDfrpa8%2FgPSnKtiga2oMJX9oT03r7E4H8nLlhCon1yTCw%2FRmB9cUVmdGgbluR5fi17ChdlY%2FIpeULJ%2FemHQewSTPW8uWV45g6T6%2BwH2aJV%2FXZDP5DPxsxISLXvzyxDog74SjE7ePOexj0CULUoXmM6AE6nGGlTD7yUamawVPTZVv9oLdOs1cfvGtbyUCLyQvSl840FSO9oHf3kv9acjyz2nsnrh9BCxM6hzHaqobvdPHuBhHbgxnxGyP7uYli1pISSm9OaCE4X0iKywUXmYOGID01cJzspLcWPdvQh6LID4bIM%2F76ilEg5nmZX3AslJ9MrnegeqWvvonZ6k5E7JivuNg9WaB3x6mEBfijif%2B9MdTqqXnKyU8XPVr05P1Bd8gHFrmbRHCd9wK4sFs01HIwMIOMyMgGOqUBJ8bhI3jFdbLYHHe7p5sSSyP894Se7lBNJA4adBnx7qEre1YwGM%2Fq0rkoOPcVamtxCbttIf9FutvJKgyUlCj2PA46nMCz1e9awq2nHQsdh0zibHzhR4kTtk7u6Mu7UGH9Z1VOGpKqk4BEkDHWRTX%2Bbu7eH8f2vnIAcDQgBGJmS%2BFvWQpqU5q0uEpAe0IOKsL1WhfniukFUe5nEpB2BZpmYMYCtMF2&X-Amz-Signature=f596a2c16d5b4cb3e5b146a5bd04dbf3ce711603800db2ec996821f7e75c055e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

