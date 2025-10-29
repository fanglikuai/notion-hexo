---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUGPYKH5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDG8WWsSk9duX5dnQ5VNcgDdUA6L2etV8XuUO95xi7oFAIgdM95T4T4u1k%2Bpuh%2FfeomG5LPFby8Bp4RaKWwRReQ%2FoMqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFBxqoBIRHimpcqQPCrcA15svr5T%2FrbnBEMNKfjUYVGD4BIDqBsd%2F2tDu5jkEsuHsBJxS%2FI%2BZ3ULOwbCywTPbLu3w0E9IBMX%2B%2Bq4uQkdbLRgtJ0ZuCBNxKeRygk66Ya9ehxyHhdCgK16%2BiDZb7xcqT%2Bkdg8p9k2NojaPdV3FSHdTjdqGCc1ebr3SC7sk7A6bLsclwiivPAqoFmtwwSpDBmuxxr13MHFkKiaHyx3paM7WYolFs7Htp9qhCDgv19f5x9PmPW5m0rcMJPLWfD8hQUm9WL%2BvNgnU5dl1Adp0mYSM9UoKuCTRnO%2FhAUESZdBfXJNkWF9Ef7dNBv0I9slc0B08oxrUYRjO33n%2FdyXbZDCqTHYKm22qXNKeQG9j1LzQWCObraZ46QA2nTrMs2Zh56GTK7QxaC%2B8zvFDS0dh0krfSREsBi6%2BjdQbzOG9oA4ejqyKtIlpongq0jib5wQqiFyGkdDrQjpOrZHuWFcQ2iwAvBzymmC%2FhULInbDAnXaiEakvuvhC2ukhhWx%2BJsJD%2F%2B0Jx8ZTzrHKqzsOiUlM08Met7ULjSwQyx%2BZrHp1PtNe%2BQ1DpnvF84UqXEp2TJwZUi89oZZ4fWIUm7jBqPdRNLTGhqJat0GENxRVC8f9UKkjNfT1jTSA3dUI%2Ff4%2BMLScicgGOqUBEYProqdYR00LhVup8t8FmIWCkEgkulz8CHZvSYya4C8Kcpqd%2FiqwIxmwjhVMRAcmzeItOBFLj5oeTGKau%2FlNHWlyujiZ0u47A2WD7N6UtJPSW%2BDp3nMwBgORVSkYv%2FxI%2FdwKa0GApGy%2BafgwUwdC4zCEfh%2BftEIgydi%2FkLVJkjUoU7LlBzwV1vzgjIf83iRUEKguhiazrTkH%2BWC%2FNeM%2F4ZKrPRDk&X-Amz-Signature=fc3973c7e29d474352e00af9ee1124646fb770421213bb139cc07fb261ec2d20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

