---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VK7ZBI6J%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIHJhlcgPdMQnUl5wx%2BxOBXCtwKLB5NFp%2BZjuBwxVP52pAiBMgwN3vwGuYE3vCuarepd%2Bfz0r32nEO3URBB2WE2KapCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMt4U3f1coVO5maE2OKtwDd1iVz59vBnGDenYkaAE8%2F5lP3Ss5Cv4BWX6P6Uucjjgb6YbPxuKLaBBgtueZPPL%2FCH8J%2BB5CgAljBDsnG2so7iXh6BUlb5riSiXd7acLDB6vdPH45SS7iIxl%2Ft1z%2BL%2FYuDbRaf8ireXAIUXPMPI%2BkV5l5PrOBqkJP5A4%2FC3Dw%2FU8cOr4uByjqu1vHu53rhfdAhUW2Ae1d3as2D5rlZgN%2Bs6SdIXmIuqGCAc0REWNymXn4XgDAVKNFyjaQHcicJVZi5xVdDxT0edD6Hz6F%2BsKj3VH3tHGaqhrm9%2BnlThYV84izTr%2BCyE8P1oqd99dMfffyFmUnwkozlWGonXVcVmZ90pFx3hrGJDUTEd7qncPtnVEK46lmiWaohFHNRAjwCb%2BcA%2Fqh%2F8jY2lp48PdwFs%2Bo40dOArpAHgbKKNI6JIa%2FfTSqe1wvd%2FFCszfynTrulAcE7Htlr8yUb5ty2pH2tNw%2BEv9hlV%2BZe8CihKDgwoB%2FYTT%2BfXWMAR%2F1vWj1DKiSCeLVVvVRgQ1JBFJvlTqR10eZqsu6PwHH%2BC4zA3PUDGuVzGLxlD8sxIxGrT2Ux7dFs6XXgw%2BbnVrD91BSGwmuZSCu8%2FvKVaWvRgRtNUfq7JvR0Jh1V8ZLeIKXd0sQwgwobuSyAY6pgFbtXqLCqUyglOkcWd7SuPKgRBjxSRlPlZz4fFzp%2B0bBYhfClWWIFbBSWAy1w7DMUD9K%2Ftq9o3V9M7kmnrMgvwcgum9TxBRDSIMOgZWPe7vs6f6JqnzHQ6DYa0mUrq00V4%2BaPWg7lQM6BAJpgGFLLUdXKtcJRFFHEgtwVxgWXkwEWObwcxzDtUXHw%2FlS7ze1dsJv7cyBDCruwPdngo68fNUaQh9BL0E&X-Amz-Signature=cbdac72129db38ae59efef6bf6f9641b2edcab8177c981302d6eabf1785b70eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

