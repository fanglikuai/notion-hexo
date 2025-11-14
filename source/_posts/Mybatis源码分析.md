---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWZF4JZG%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF7HaATBXnAhNYhWMiDQM%2BFB%2FFm6vfyr0AlrfUeQLhheAiBIThsv3EVkFRMniwa7oYmkXOe7BNwl9UPl%2BKrARTModSr%2FAwhcEAAaDDYzNzQyMzE4MzgwNSIMn%2BUrRB00d2lLFRkXKtwDoURwnKaUZ0JTj4SG%2B4eHPRhO9j6qBTJvBlQAKRlK3P%2FR4BLDeZDxX9nRta6DTJI1Ab5PudVBBnbyky5P9sb5dkdsjtgenBSpp9OXJ8%2FVAKscfaERqzQS6L1ViFXNtbDi53SBEsPHcpKsRLdCdLv3Euxtia5K3x7xckNG3KvDaGJB%2F1x970N9gaFYH7bWtbH4HXYE1gGEE88yC%2FYr0sLjDijtFy%2BmURxgbjIt%2BaaOMGkO2lE97Xy0%2BsnyWV3GpAVmFN5sZ2ZhPR79z7FQYlJAju3BxDbLCRpUh0xbHSzrpzxZ9Lfv7SWIdoiHNDN34NRfrUUwYVYN1sOo8%2F75mgV5jqJUfOollWm%2BxaahiPfs4n5d5tnFAU7Q4U8Yfiu5eZD%2FsnF7aXxxWvWrZrr1ZSojwygsPhV1m5PZpZswXKMQlJVUJ3nNcukcPSwsHDkc6N5dBwFTTiuwtyoki02EaoifhoCD09KQ%2F6Ef%2Bc%2FhffZ51F6TN%2F8gyUVayJgfAV%2FPG9XM5%2B%2FZPs7ff0gw1gTINJRgz1gKdfZs8JouAOXGUEylVPgrlqpq86py%2FU73W3ZSArWtEnrimR9Kj2eShmJ80FOZWQoLefLElUoeIWR6DO1wvfoX93wR7pFB%2Fa8YlJkwtLrayAY6pgHAKJ36P7k%2BFKK8eXiL5zUodgLwwEOJsmgAPvlUAOcbIGfTMJ1ChHDHRPY5VGZ6P7LRO0MJvOcY%2F%2BC7vh3K3zk1L4odcmGszLOHD4TJurUr%2BgK80PfaP0GP2R6HFc71dbulFvYN2JyBi2kdN%2BLXqshvPXhfO94%2F7%2FbCwWyteEcpk1IVZ9rWWFmhvGvvbaL8M5kodRzkBuUA40x%2BL999u%2FYkLzzp%2B4nk&X-Amz-Signature=683b30c8d63a67d3fcb2900f5db3cfe3c603f81af289ca0e978ea3daea47db1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

