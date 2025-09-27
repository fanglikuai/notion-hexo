---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLJ74EA%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIDdCzzBO2ib3BP2%2FfoAvZ1fXEuP55iLHG1smz67JoBdwAiAUTkafr0uicsaSTvCXI073EijOSsJ1XxJ0mFck9CPBfCqIBAie%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJU6fefmAVMjMr7nxKtwDEgrUn2w4DSvvCwMf7E1H4OqZCGB8KmwaSOSOzdfym0Wbae24CI0R%2Brjy3wSARkZsvwY2VRdfkj%2BfHkKAIFMNSeqLbUBLFn3zwFBKRHzS%2FBdskI0GeNIYtJLGO%2Bb0jG2r5wnAJB1fAci07V8uv5HBC5ddGZ5el5jlp1eKcJuDTv2qW8K%2FkS%2Bbk7QdcWMPUzHrBdeRP7f3D%2B5CMIZq81Z216%2BGhgqg8jVs%2BZvA68nAZRm5MMp8RwX%2FC9SO5qL1G%2Bj32E0the%2F5%2B6namp2prHVzPziAmQ%2F9vLggD4ioiVKS3Y7zLzk0e332lgHR3UlGV30C1Pd7oxbgVJrygrlKdPb%2BWN7N%2FVJ4rhYr4JJ2jSpu7qX%2By0dkMCyQH2EB%2BlsngGuuOhPZv4%2FVS9IoIvvYuhe%2BzgpwkYYhdvvO%2BWd6GZrzxyuNzjzmYfB0hxjhjbthgR6vctp3vhuOFBDvivSAooYIkyzKcESiJ00rHRQ2fYoVAthsh6%2B%2FUcLc4lHRE%2B0HMC%2BeLLhDpbRwkCWtCaooVKi%2BqdSdvXT2yOE0MlH6vFmtriUauF%2FouWHdYaDpdT7IqkwxnChz7yRtzQFBI%2B6HLyNsX5lM6%2BG1Isa5Isf9Rl7PcvMJJHTP%2BCMIZw6w4HcwpNzdxgY6pgFuLBWqN5wTpqaZuBIVEvkygGuwNdUtq8D3fnB5msR%2F67HX52uimOUPEUBQESy7L3Mecjf5LxZXl6GCOtbctZOEbw4l593aJVZdXU9UF2oPbA352zzjlEtkNenOVzs%2BPTN3Nv6WusWQLxJ0xZ%2Fe33ZTwHfzfpmpeWfzF2l32QtJFrmdkbOmATPw9FmOniAGNFpYxw7IWf%2FJpeWSDJRyod2%2FkXbcV1p1&X-Amz-Signature=64a4b9287f695a0ff463929fc9e35cfffed656137fd9dfa47bbbed56280bec95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

