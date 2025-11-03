---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IERB3GO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpdIf7WDkPueUa5R9wkhtQbO6xZbUJYRtY1Xr9ORRrzAiEAl0lSuXKKiTrti4YOqOOLqDUOOVy9w1LU1lmAVRg3PiIq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDBs4T52LSD6ExtL0xyrcA7mKA%2FNk%2F%2Bt4PNHQnx6Qye0m1%2FykXdfmE1Ouuo8AiJSghA%2F4AtGhrc62YWtSdK%2Fx8wGqjd%2B3%2F%2BJLARTIiXZfoaa9XPaYXYZvpxx4KFlIXigMviwLYZ1foVnsqqQsgHrGiELw0vaMjUWpZ39t5nBTRru0Tx28%2FFwpdD8eXVtHjbFkNnFr2c41v9R8Boq9K7CUy7bkGE75Epvpufb3ndP3Y7q57tjlgBpDSecXmlHs0UXxmsPRfPETfCKav4uVSv3sLTM3Wy4Qb2s6PDaaGBuMUpHdiDNk%2Bz%2B%2B8kwfQ9US8%2FvzfOeMsFyBPzadCd3iMVEtlpXOvAn6VNwEuOmKQ%2B4h%2FzzT82DoWhw5ILlFfyfMnW4Wr4CamGwiusFT3xwfGRLZxbedlJPakZWsbhwQwbFyD96ztQJqAXtKIaP%2FlNjx7ta292VpcJGhrfbpB3044HIGX1nPRiiCAtI4BXUYSRWDmn4TFZPx1P8aigxNSg77%2BChVeQKGXZBhHrM%2FtwOasj069W1bMgYhWfKpbeL9ldSfaqILH1UCFeId9vYXgDSdtOnnHWXROsIgVv0eAii8zEFEzfqT5zXlppcbR5H54vHLaxVatPx9vkRUVk0%2F3GyXttSXJm4AalupJXPzZoA9MOLDosgGOqUBEBKB3TPAed%2B1wXvYVNqw8lyoxkVBHHFii%2BOgQJHXmbsVTRn0iounXroUV510MngqplWfqs5jPd8%2F766Q5HVSFZp0Tsyjp3OL1%2BC0aNfUslzyMgJg%2BdFZ2GC8LUibsblnHelfx0jzi8%2FHUNvJoc%2BivzZedAiEiLVQzxci0%2FJ%2BBbvV4irByoLUFdiq5seSp09IxMQMcqJrCW0WUBsD3GNrvi%2BEqgfH&X-Amz-Signature=30b557a6155f9f0c17d32a6c55fc067e518f3245f05f88c128d6ae3e240bef14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

