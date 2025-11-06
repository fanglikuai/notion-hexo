---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USBCQ5JT%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCzBe4gNq3d8aBtkpZTGc36xTc7bXSetHDOJ7hjCFvmNgIhANYknq9D%2FD6jTfiWSAyAjMm0gE5lWJ95CTbnQuMRXOH2KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzsIpWXaJH9yfX9UGUq3AN%2FoOQBPCojYXOBHm%2B0Tfx62aqgi1Dje8K7GD7bjCXzij3RTsvyCclXEU2Eu2WSFMY%2BkiGMl%2BT5MPu%2B%2B3QzrZL8Q8e5vAQHUzV%2FHom1vfiy3pWWZpROpT4GzU6z1ntL6zoYltytvrV82Ok08soN3MBm1E8r%2BoYIzP1drtliopf6BSGr48yNGDw5sqNQuzj24maBHQM5wtHCOkmYhaqfdS%2B4apcTJcCqEW%2Bf%2B4tG81UEp0aUPSJhLuFAbNa3XH1e0jM0mSf4l048hfz7vTO0r0OgnSc9mWIQeYhftdM7LsLIXgsNut7Z%2BwNxL1ulNupk9Uk6XGoVTfTu9jA24FLjCstwSBuFlRvoaeOYorj2gkhmKC6QEycEzsEqolWzR4qREhNB2Jlue7GyBWwp9o1xOxYs3gDnXoaQPp64onTrMkxCSJpjIMklDi2IWnPiLeuPGvQfBgve76JfbTO2Z19MISBThyOSyjVy4WXg3Gxs8fHCsDqczuOocAddsyUHU4DEjeAWMFB49N18gxiArjOAffIwvnoRPCn3me47R%2BHw4PCJ1Fz8AuQ9GkMO8Ye6XbmUkvTQF%2FWRLlmZhdNLvrrL91WiwUjDJxnEPxW7qSEZhOr9v2jGOoDblbkR7e2twjCp5bHIBjqkAXvq6MbiwXQZp%2BxMvJV0mayjM5tLoWVm22mJkQVT%2Fxwp8Kzt2iuWay0NAmE0aC6ISlQkvMHWWP5m8KW35iStLCCyKs%2FyECIV0EUcq13ESd8MZQlU9xc36cqvv6PplDASI5ozPaYkXf16dyTaeiz97mJxM5TYQcjWLHhiagso23ywyIIXf8hF1TjSliihNBQxRA%2Bv%2Fr%2Fj0nkmgVNEyeqflMAixmvF&X-Amz-Signature=002854c73c7ac11d5eb3e700cb6039560deb58fccdef3b4041bee6b9f075ce47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

