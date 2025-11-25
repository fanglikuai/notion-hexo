---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ3MFY27%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXHNYfnlEUCoTYve9O2fRmLyZzJFOq8JydxrtA1vvYpAiA3QLf8AC9SiMrNmg%2FRQmRad7HSZb5eNq1K4xfMvpKj%2Fyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMyOQzT61Z8Hz7XHpHKtwDqru6ksWW6%2BHB8TK0Z8TIPpu%2F9BSh%2BNteD9nYpvxQP%2BBrBrXpBSsMY1qWXl3IYzskTVc2U829tJ%2FDkmLZ7pp4DXxxNwBrjHXuKXQ%2F%2Bv9%2BGBEp4WSeYOBZhgR63y2Yt8NPBtEf0waDPBcyEH1PMm4Kb4%2BTk%2FU%2FjvNRNmn6LZy5M2LGlYDGuvHojq3VcIwjMbrq%2Bk5B9BRRDn%2FWuvcb0wenNlUjPoZS30qYlCXRUEkU8ZIR2nWHBJGXNjSbqhvZ9fKKHTuUc6F5ejKUnL76c6I0Rgqi%2Bv3EB8E11UfYquMhuCiZaARn9YzXwAAJdLKKZEEV2wd%2Fy5iRvTRZ0J6N5HXbTfonzkIwlLFb9Bp8ngkpomnYvegz9BsxxgNa%2Btx2YLTxH7jp4EgHXqMLbPGm1nUkx9dEicQ7PUk6G%2FRr9mW3A4wU7V5c5UZvW7AFF%2FRknwXB0qDLKXBEoErxL8NpglEFNfhRBnRReHenagVMQthjY7Wvo7589Vt4y8gaPMu%2FlkFJx2d0doRu5ynSiApjXnmiOit4roSNdeD%2Ft%2FoRheuwTkTGEK32di4ri0zXeN%2BTCt9kDMmjKvl%2F%2F2kn2K7Vrx7LO1UBXsdkNtFAPRg828WQfi5SyUWx2K5ohtPM%2FFcwwLWWyQY6pgFt1JeB8KDLwEWOvcrkJvtfIfIjB4JJkzDnooS%2BF8%2BMNZWe7QpMjO696HYt3960VLfgj5RJXbwYlWPls867osAjbykO0uhf8OKonMYcMqbuCmpqJAUFM4s0pasq4lIMmyyfrGhF57lzXmpg0IZhvSZQ0sHSNKegRPNuskAnL5iHQZyB%2BOJIePitAJmL00babwfCq1GuXyjICv4zD2iZTdJNJjpT%2F%2Bqw&X-Amz-Signature=9e138573e77c2954631cbf11cf6bd47c20b00bb8e3661202fe5a18364d2a38a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

