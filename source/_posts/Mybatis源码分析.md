---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHCSDPPW%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICIRp5wG9cYlJir1nG6phZDE7H1FdyxD9n8Xuxfg91trAiEA0Yp6bIUKou4wkV7Y88H3keR9g1N1nwadGb2RCkJSmysqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPjw2sQHGlG%2FX%2B1M4CrcAz8HM%2F0HSDRi9ygN7jRaHw%2FCDbiIjZP3ATNvtKupqoaS2rYxAK5%2FS5e%2FGq9T021ks9aYrtd2ZFc3XyvhyY%2FpUkqkztFUD79TYUhJ1TFxHVS9ERAuOKn9DhOr6%2BVO3z0rhPvlZnvAVEezzjCxnldxIgTi0yWTAP2b%2FhIaPnbwbAQE1CMG6Y%2FRYDa0%2B3ZsFY0y1s%2FQAz5P23GrtqiHNPfE5S%2BCEQlhH%2BEhTAAec5xovIFpyO0%2FpDYzhopki4KOM28qSUWHpDChwDG7jEK%2FsIjmfyWRGS7s%2BbNXC%2Fhhyfe0rC6n22ReMZJtDrc6rmgFk%2FXieew0WETJtM1y556g1PRlqdVZb0jKM3sOJfeyEq2PLN24Rei6iW5nZdrC4dVq7dOtsDFX78wWDhmOf%2BN64cLRiGXIf6zKS7KUo%2F9Nsc6Lhde2zPB6dsuI62gUy3DITiF7y5eytzwO4forBVVoasS6KPDqkF0TtyulDhxQH6kUJMthEwnWNAVgAnRcwqW0EQ%2BQCR3ATiKaeZRDs%2Be79aWxOATVOtchNJRu3vblTGSuXk%2Bq7ndfB75YFHwQlCXQHX4IccaQwcVRWUXMNHdPE99cyJEpsgkc44XJSxSdkP%2FGiAQD5tew6KEcbGL%2Fen6RMNXe58gGOqUBA%2FFpLeX6Cq9cxCZ2PxvyJFDpXVSCaGxCAPskS5G1qLTqBrj3Uco87pabQkhaGgLV5jV8TpQYcLNTes3s%2Bsi5p129n%2Fc02hpMnmQHhTKInUg9Iv9G%2Fg4L%2BBOzyb3hzfdr4zOY8eIAlHVWDCZkwyV0pmziDjVgoWBUHUGsnwDzKwqVSt%2BPm8IF00zejb%2FCb8Ul6zdEWhY8J2HwSFPodSHoIreZ7Ydf&X-Amz-Signature=86822bf52acce7f941103562810a3cb428b043059d12d85865acf89782857f32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

