---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VD3FWWOH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGrb%2B5gEtNQsU2TYI7JTsXDq6wLe43QAvE%2Fi1qwIoaq2AiEAluoHwtknOcCgCwuH7o4RqEPqll5nVvgtgAOmnJV0EuMq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLMQS1U3%2BHBWPbh6vyrcAyeOYVjVlCnGiVndHrPcE84TYtQtwHzdqCDs4XrC3tnUbW8advh4WAtNUbKEp4b09zn1qeq1m3vcAjwhE8d1pIrRxz1UHVRWDLLAG0yuWNatlEn5GY8NrdywZsLNYnZRmdNxotYrScNnJhtG5nUb%2FSTC7aT%2BiEDiZNoYzf5IEsYHr1NyytMlMjwUknxC0%2FpnKxNyGXE7CWV7pmeNyLRjVvU2hEZJz6TIpU8Jo7UlhTm789HBnEGvZWVZJOID6dy0tqveyPW%2FmIbjhTRNRx%2Fx2vb5TSVQ4IJy6Ft6k4mdVDkfPh921J%2FmT5dx10Ei9y8ozE3Fdlysuu6Sh%2Bjt8TAuE5jF9JgcnECtYujPrSlGAl9IjfYxpC54SM8xc7Ju6jon58M4%2BsOd2Ry%2BOvxFBk0MKYEBSu3HK8nhXaFTFJqQYb1UhO9u3jHMmviyuaAuoAQt7N41R8guZhQ44v313lpf%2FC1iTxijOl6as4%2BZCwxm6ID43UftLIFomLsOgAnqxGV5jlxjzyO%2BjvbWFlFZCJ5z5kA7SkO34kEnSW%2BnyT91EURsUVNCIDjvOFSi4CnPmpLFmG48c0AOouow4xkHeWzw1tlOuDBlgDVB62ZSVM58jFcJpbbEKseI0Q7XaoAnMLvy3sgGOqUBLeQBFPRGQ4cZFZP3T%2BrDJx6iCYiHA%2FigJDwORsTc2iIkoPzTT1h3iQoaoCdX82nxQ5z6l%2F78d5fvpAt2I%2Bc2n%2FFCA3yldKF2JaO16iYzWCBrickkQYAFVitqIfzAmnhfdua2m51jqGD5TdPUd5k1zISOJ4XC16ILQTaA0vmoVm5aBB9NO7pFXno6btPbo6ZRJKP8F%2Bfk%2BKKXb%2FC1SjKOoEtbE%2FaA&X-Amz-Signature=0d533dada1f8f5194fbc4ca9d89616727048a5f7747285fce1d3385b0f11ec8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

