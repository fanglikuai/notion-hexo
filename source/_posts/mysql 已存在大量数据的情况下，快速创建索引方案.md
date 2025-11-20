---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VC4J4R44%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQC%2BU%2BuoAhejYhQ1NLUl2ydVg0ZjLPL42e2RDB8865ysswIgPx7Ltrtmyn4BwH23TO39s4TV1RQYcIgxLfuZttHJVI8qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKwOq2VtZlxU9WCw7SrcA3Dczt7oCkq5aSRON1nX8yhv18XrFNrEydO3FPgQZjTFPeybBH0yFyLdOI%2BZAYumleBZAwfbUS84GLjxnNNe6IFvsyP6ayj3YUjLTY8ExyojsXSZaUo732N1iIU2Tkpr8ku1cBvADLsJShpvfwtClpV4M2J8EAOJ6DNuWltM6luNMhk4Ke%2BeSIe4RIK7Jg7uuUiuoTbpJs%2FauzQj0n81TE0WwVlTyyPlMhgzC%2F%2FZ2fyEvRbiRI1ADHr7hAQrCOsI55vOxrvE4MF7nXEBI2NcgXPd%2Ft9HjahVkU9u9cauotJ%2Ftr1%2BH7PZ0G%2FNJoLHdeMRL68sb0NqH8McHW%2BArtF%2BwwgZPCRd39vUJ1uAANiPEio52%2B%2B1uySWIrb%2FYwCT1G7oZzpXTLwdznu%2Fc7c5KW8WQlgUfIDxr97j3zjqx8XkbtWgnxxI5BbQVjigkpMZEg9ugDL60tuH4ygwPEX6X5jBxeuJnCpKDNaej3bGEKjvBHVaiAyfUA6XS9K75Hvjxln4qh2bvni%2BXb4a4fFKpxh15JVuXoVyBYVJz0Tqr%2B%2BkSkVVbwwbuylQjwrOJA9y9O60QrTYballsk%2Bzfk9dtDZLQm%2Bghs6Dwt7MnE%2BxqCPgO7TvtOrhfJ9CgH42Q6WRMPeS%2B8gGOqUB9lN9wO6VBrCudpzN3cPB%2FzMmkfW4CTnpG%2Ft33XWZeAh55O4aWRNiP76nuKDB%2FU99QGkrTC6XjnAbRwTuSxYfVDKgPCxnl3c%2FUm23GsoASl6oPehR30YGm%2B7wwXMusEtKOblBk4J2Bul041ga%2BTwqAaiyodEV9QBvfkhc%2FjSOkM8NJv9Fw8224uKeUp3w0Mq%2BYcZ%2FjwFW9sZBtGD0tvEn%2FrzT1BTk&X-Amz-Signature=7b07b16d358fefe89a9a61bf469447a761163d45eec51ef19e8609692c4338f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

