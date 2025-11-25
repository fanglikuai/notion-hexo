---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B76NLCW%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIExxAxg9RaqEmoWt1OBpXywNUcbid9a%2FeHfbj0BU%2FvaLAiBQRCTNvSpmGUBzdsoYsBF8QuAKXnwA8JNYWplKlkjmHSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMX0dWJFVdCUQ%2FZA35KtwD0brZczQ3cn90yFlETucoPyguHJW%2BqeuDKABIJ4WXiqtrCltXbHvi5ireHv3LvivCLj%2FmKTtwulyujs514buz77snwU9WX8fd%2F9CqttwNtwJ2CuqlNO2ml4VFULl4YIvNHYkiLbobpTPMCRr3qEN3uy3W%2BvKjkcxVyWNGIQ%2B%2B%2FsEAn1ISatNNXjW9YO1pOJxb2H0xmxCni5dURtLv4ga7fnxUcXkEa6FOuteiQ8uTC8cEzbJObOu7CxaMjW%2BcRmqKacUBTOvFJpUWKxFSOn7%2FDMI27%2FykT%2Fz2RZd%2B3a57585hihvBvxqc1r0RyuqLEUcYJVMtg9e1YKV4yeumLLKnxLtL7XJnw2nnxWGb%2BCdj8Js2AkidamPmDh3%2FrmOIPVjduffGgPCDGK556V1gCNcKtbHdJZF7U3MtWgBRVZuXEzTLr4vkFpkuLMkobF%2BBA4y5P9fiQ7s1Y43vPuAPqPEJcDuINKL8FQmta%2FjaPUK83Fhh2rHqir5eBqlVqOyFvMk1AB4SIXlzs0J5%2B70Gs4FpgUkE7%2F7Lq3BTn1gnJ%2F5o8mL6m7zIpOVlKFoPVh%2FgtLYiKPzN26xmFQt1tjha%2FrxdzMmWE8Q8%2Fqswf9LBqOr7bl9w9BAW8%2FnVqQj7tfsw9daTyQY6pgFLDFreuYSSBupbEXjA%2B%2Fovlt%2FLvS%2FxvUiWCN01D5UGQK4bgNucpQLh8nQju802cmkQOfqDGKBEjCrmcAc%2FVlno6vYxlgcVWxS58KyaCpRgdZ29%2FBZYjF1u8EeiF8dOMdA3ZTs96FY08D4mqcfTZvQeYlZ3veVL6gA2Dyfen3CrSIrt%2FMN50CpR2w7xKbg2vbjEC8fHF1ggUF5z0%2Ft88b8uPM1ctqc%2F&X-Amz-Signature=94bb39a6b55f9ef37e04de6f2c6bd6eebc6ae9b9e27a99160289972a728ba437&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

