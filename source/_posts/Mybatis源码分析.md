---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5FUQULI%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCICuD3lEzrcFGOUPsJInNx9KnoH8jpQXysSpy1D1R41iCAiBCtS2l%2BrOHp6BqL%2FiwfHQpI25ykaexTDI1z2CvantDpSr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMRxjoyV8k0q%2BCYMxgKtwDE5a6%2B1ZOstyX%2BU4rvtWQEwCqKIyMUSl%2FLl4tvMXDzn7jz7jk1Z448OV372sAndHzJwg0vx66ENOJEkj9cNlSm2PnlMnfiIFnSYN5WXBDk0uMIVdjER96UXawrv%2BxWiSl4iibmdUpdF%2BcXMKKfVC5KuElpIh%2F264tUCSAlIgBVHSCvRbhpF7Gsq0c2zU1GxSCI2a%2Bd5Q6VTP9EgROww4GyeKkGiaYfKZgzdF5j%2ByGrZll2SQnYp%2BoqRWJzkVh7QDGgL%2FKHqMCeFHoLecnzS%2Fyl1zZ1Zg3VhLXc5whBStEJSm4pciXqkOJcxpJkUeEmh9Swm%2BXOPbp7UsVPYbbIJBEyu7XSz%2FNrtjF5gKjySVMRfTSpxvvqk5cgoTY3mih8PNFHoMGzIPbcY7HtDd8ZG1xvzfnIJXS0Y7SQASPcrsWhr%2BTh%2BjnoO9Pmf0ywoe4V0FWW5HnptgIhfV%2F%2ByzEfw6Evfdm%2BqyVLjFBkoEOOXZhI5%2BAwENT8G9wB3nEQoNE4Gx4d0PMDG6lFHC4Yr8snzKEAL7U70MAmwUPRWHSXO67QB9JFpAHLpwxodvplEh5GbD3VNXuAkMybvpW67Fq9rG4LPoGRR%2B670f4r5LyfV2cq9BAd4hqz%2BfXBlASwU8w79CWyAY6pgFHSlCq%2BAfg%2F8%2FVYOrmwS%2F9mQSf98sSZ4nZohWTlurYaySyIDKSrQsJs7hRONHoMLRVHMBj6BSJplkjnzJdzGpR%2FHFZLBeKqm52CrzFeUsKw3enfLc9KVkNujDNhgFF59CPMk4ok0lbOlVcBdamT%2B1VKN4L06begJUpxMVLvFdypU8WZP1GTTqFtVdymzA9A2SWQIZ6hMkSdPjQSaH2YvX%2BNItmws9z&X-Amz-Signature=ca025dfe71483950a28af7dca8a74d73dc68230762240198d392a2db7c631e64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

