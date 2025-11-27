---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMRRZYO5%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBm72T31ZAFDXOkOni%2FcAe6jubnXWnhdEbRkE1XtrFslAiEA1kYXdJ8Mvdw64V135W3g3aJC2LhdEK2y1aDTbMBqhG0qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDF%2FMaaYSTRsPGVOgircA6cj%2FLD0O2GifmxORFahwFEZBjTbnloT5YwVJY%2FjS%2FZ7O%2BtaGWXx702U9sYKDvGm%2F%2BMKm9ucMIRI4zY8%2FFgGeBq4N6P2PjP1u1BPuNhMk%2Fztz2F8fdWfgAyCxFZEK3pZanQ%2Fr79HuPxktYMoZ1fci2N%2BHccOrpEGGuFP6QQAAvA7MsHW%2BaeCBIfwWXnN0zGqiMWY1hzD3%2BwmhLwQNZ2p9i8dCPKE9Wf6JJLjcFBfQnxI6EN7%2FQjFK%2FWSNzOXYSqNHUMTqKzZ7eAZNBdQmH7d%2FRP2%2BWXywrlLhU4jRTeo%2FVuVdZB430GV7nS8Rwg7vr6tcmVHEUhIrpA53aW6J%2BBMo1CmMh9I8T%2BCN5AygTQiiJwwpDf5YgdrOafoyTI0LrRzEZG7dlgPr7mjaB2IWa77%2FFlgcKJSCciR2wUuuZyBM5WrL0pTPmoxOlBOCK%2F%2FqPPwwb%2BNeSzyWM1GgIOVepGi0gCL1RyByQP2e8nH3kGl7zTbxm28i0q7lUQoe5ZaCumigq5Jtly3ixAeoB2mNiKK24d2who2PGSU3dl0lJ%2BaWQtKHBO%2FPuEoYf9j%2B05FDMucNabm7TD5ww8QooG2O3wUrsJEv9SQQPZxsGXZ5S5r%2FHcOu3nmjA4IaSsezsc0MJu4nskGOqUBjniCZiku%2FJRJdzU6Jrxx5XGJCAPfJMhB61nT0PWBJN2qnqZ2pAG2Mszsh9eRgD1hxLBnAywDMfdPDkBwlho8aTomWsmWyZQZ%2BMLv8%2FSEcoPxBc1wzzUhGp3evN32xxtb7Y1kbwS8PEOG7DzWC8NS9%2BKf%2FxJbmq96C%2B0yZnosBsIwYZwwGtUKYA8XQVX64YqAt6jvQ5CdHKFaasBWoRdYd9v2sYy9&X-Amz-Signature=748684a56f375c0e624dce55be1a10f2f04bd2f57f3429b3c656679c134e6090&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

