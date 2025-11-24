---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RFSN5QMA%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFLvYiXImkR4fzrMyevS9xuhbzWYE9JQycc0Tew7GVXWAiEAkvuVApatXs16yLhioiC0MlHnR%2FfzeJyxBxXcwMojYicq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDKa6wqZ0AVXt%2BYygrCrcAyKRIrMHV1oOfKyDFIEmsMwgd2tXO8V2vXgbTIMIV5%2FMcIq0%2BA8GouiaQPGfyqy7ZdXqFQS6%2BAaGgLgHtjHmbRmgOIGjUWt%2F%2Bfft8aNl86zRbC8zU4acHXBrkC1XBOHOI%2Bp5S2BCzE%2B8Gw9qHxCiGdoNKeYTiZrrjB%2BpyEntwZuoJ4B%2BmcaCG6Unjy3%2B4O%2FKo9BAgdRj%2F9F5JUJAK7vlsJ1sdAPJ3I2tXw%2F3sgwQ9mWJhRieZqzDGVLADhfP6jtEKithfTIKwYbzGsLY8hBY7f1LuhrXZ0cSn09S7sSz9VvDQfUlT4hTB5vXbvMrBxTJsMPLIsYnecTPuRXEm4IhhJa041eVrXWTq6FpRfpmLLW0pYY2aDdP7Aa5jRL9ruvMYx2OH2WAdHbv%2BoynTIsYS0%2B3HAeLdm4CagrsMVNtO17Yz0KIyE7tgh8CIxEk%2Fjx%2FJ22ZeQ1NqjEes8Ed0rqvSjkR2Lf5iIH9ltEyNgoTT2cOYj1FuCcMFVreF4CkjcrJ6lyiPfrTsCPeRwci0h2Hql36aFB2x7sAwtlX%2BR9c4VsTeyxN7cZR45xKMlKjL%2FCdGzG4U1wzGVf1UX9giVtsvL%2FMQ4WiVAUhtsumn3Q%2B7o7CViVdFWMJwiwepvC0MJa3kskGOqUBdU8ww9ZMyASXf0mVnn%2BO5pUpnmwJhhSBV03QwLeVhxMFGCVwTJbwFsyshTJsiTxT%2FItTaUnSZxFXnh9TQB8B%2FcHerm2XyCtdf%2BwrIQeKgpyFUHfHU30HeEYwKfS%2BHjCc1lJtp%2BIL5iKF0pP%2F6bRhcHA55jGZg2AE2v9wUNusFZO30i7WUnr0mh0Oyu9Salmv9o39dMy7cQckDhkFp7jrVXsOJiG7&X-Amz-Signature=04301ce446cd0edcccecc8d0595bee84697e3e8b4c5eb76f48e94bb11f7824c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

