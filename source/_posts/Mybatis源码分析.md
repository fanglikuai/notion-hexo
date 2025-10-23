---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QP5N6TKB%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqRlBmgg7c%2BhN%2BJJ0Qmn%2BXMQA9mehDi3UQa4R%2Bvkg8wQIgUyAr5skWULV%2BrqiKXCUNBDofCRGB1BgysEgsg3UuL48q%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDGqv7vN37wnOFUbfnSrcA7g%2BSlJcILr1Mx9mTXPbrJ0CAttaO7yN%2FX%2BC407CAh12qTnG1LlLNYlVPEZsY0u8wSJdYboYa657CpdLO6QZJUiu3bpjfBlnbUSQuSdx5SYNZuwPaK%2FRyFKoIYFL82voE%2Bt4Wn0kLPUbjOGbXNkt%2FrHCLYBHbOM5HMGAfT%2B0uhzab6p751hzs6LlTGJHUeayKXFrEZldZdtCd6wvl3UPTQnlv01KwZ8lt7DVKAz9f9N46mFMzF0LFijxjK30WYsjndUEfuhX4EiiTHHalAWlRio2HfJL%2BAOzYwweq9UgBk5gh4X004W%2BrtcKgiRztj9%2FvTMMtNJp6m3MSCvWcCqyjREvBpPdvf17rhxs%2Bc793Hwf5ejTgMWR1gWLrztDQC%2Byva5ZryVFsnA7mg%2BH2hOgAy26kh0Lzl8R%2F%2BxeuXuADR1JeYm3GZatqezeGaSWhYNh3s88TQJpqKPzCiGpfAUjcPUUQfMAyR0B1%2FqXmuz1RugcvNhDeYUNJbmxj1OXetzBJzuRaOOKf50j%2Fq%2BWGcQKu0KJwUV4fQxuv8RRvDJTjEk2R7%2F9bk5c6CLKmJnz8x86rHTLFKesjZPmpGtirN73L0VjersZV9MXKO9Y0Ef1fCjy0B5%2F%2F37uMPQWZkWgMOb75ccGOqUBeMHmUS93RCL7cnJNqxt5%2BaKLfWwLZyJAlXablxUP86GiBKmRAakoRdQDdPZvzNaxT168%2FZ1brTioGQUgWkkNEfrpsQvhX0PceB2w0lAU2Y24zxHHm2ype5juX5efBayagpngm2nJ9gu7BafpTvjNDlafzXVVGuPbnaHhrw2l%2BLbmvQ3RvXic94odJOzBe3Fl%2FZ5WZr8jW%2BH%2FNK5nqlJqlCZGprWJ&X-Amz-Signature=cf08a6e3d0a290cf3364e30ca4fe9caa926de8ba3e3bfae0f4ab83ffcae18c24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

