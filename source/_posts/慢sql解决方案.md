---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663C3S2JXH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAsh9bW8udsh2jRVJ4wnGv0JX9ZGrvNkT7ybzHweY9%2FXAiA9KmLYhw40NdbJkC3eMqV%2Bgxn8hFYzXnA0P9JPdmHJEyqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM38Rx0CMerhJNxOw6KtwDn%2FLpD61c3e8uASyDb9N%2BhFoQsjcPv02UkV%2Bss5kWmuaYcLI%2BCkNu3yZuWg%2FgQ8zIN%2Ff%2BosO4BbyHQhKRdMkKhQVHAurojVhMsjF9Rxy52%2Bgn%2Fkimac5Qx%2FJeGVnG3A9Gh0jpFzz64wxvgknUSHiu%2FwX2KuzkJZlD6WPHGHc9EnyNraBhMP3yKA2L9z3wI%2Btb2%2BvtJFSsIRhfD6%2FHEejkmyYvST%2BLK4zP%2BT9CcogcevOo7g7H7rKnnHXv%2FNN7z66euuw7MZZBCP8RJMHzU%2ByGKrADsdcpEjuTllCfdmLpje1f7cp2yZEn74mmuoxJFC2MaFd93igUYkscEP6xolf0%2BoBX4bGpLttG5HcsA96Lbmg3vx3ucnwCAz4%2BzrQJgyBtSJ8rdInzFLnUFaZvjKZancIkEaZv%2FcNstG0e6shFPNO1yUPa9N%2FxDXClBPa1alQf4wxuZx1CGH0CUV9FjQS%2BMRLUUYAgCWwibM0d%2BjkAh3%2B%2BO2qhOTw6fFk4i7PBkJcy0kQHQ9z4t1mz2IhVMZvTYWe1c8mm%2FrvpfrcCGAo3yMmvIJpg3yT%2FJYH1rYZwKXgJ1dBE4uvkC1Oppfv6l65rQbzlLxkVVK4g2oRGJGwq5phOApuB%2BvH7WWfdc%2F8w1%2F69xgY6pgGM6yGfuSrzolfAzPVbgWqW4rEkLarqlVfxi4%2BafGBaFLS1INIfEnfJofez5ytK9Sbem9nL4omUxc5Q5lT%2F20VO0JD61%2B6zMOr7ph2DNefdyJA9AsVKlJNCp1YSeyX0nyxSeLkb3ueLRvRl65QQVGp9%2FOBYrAr71CBv4txMQf%2BxNMxFlFRcfSbXhzXjnobOQpJCiR8go1Wnh3T7BaQ7pOe9PN1buqGc&X-Amz-Signature=5861b77cd6ce5d399017eec9fd9bc4cb42261d452e46836562436ec2536a9b84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

