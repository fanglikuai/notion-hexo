---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QRPL7WB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T150157Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQCQA5PC0ZYiEVIl%2FR94GLchD6r5evIIQv1rrMi43CshCgIhAND7hKiX9bA24BM0XAYBGZMRq%2B7A4CAp2F5TfT3klhQ2KogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxs3knrPdfFcrFZUqUq3AN1kIXzMSmyjb46%2Fl9JAj96t2iJJAr2Y8hMMrc4yvDy%2BFjkPoJlqkXOKUOvgNQKWOl1OwX%2ByPR1hV7dlWEL32ceknMVxRppNcx7SzG%2BTVyOEQKi%2BEB9iMONg3KsWrKDmek99IvDyz%2BI%2BLNTmKvhP6ZNheRhYgqfJOx%2FiI055cKeDEKdrD%2BlnqngIZTo%2FgpufvNItLbwBka6D%2FcBRY3cGhivOSRyutYQYAb7GjJ849wXi2V4kos%2FPlCZhiug0ubpz%2B0%2BcfY5%2FkwYyel8zjMdx2y7C2hNHrSOzKD1jRgn8v%2FWjA9odiVxahcMW76MRiemrSoRQMIC6zJl31shfSM4jCDwnUAEj%2BgAKL1eHivNV58uWIbAk4Yvq%2BVd4Jo7U5743ADGl6sZLGRK0IF8sJwikBz4hRTbqfbGawz01pS%2Ffohar5gB4lN1oDKiSaXw2r0C0oMLwaJToUduSkHsNuzjtuGwOxmoN3cjqPCD3RYbQ5lYpD2Tqauy51R9V29z3eFXxA%2BjUrUR61DAm0f0ZmhU22CXy8unZKpqGg%2BCtS1kjO%2B9cYNhmhTqB%2F8NiXDnw0DjJh6C1j3zPVjZhYTed2DgysnEO%2Bq5Zgtdlp6kifFDtDX5SpNjGIMS9pKAFGYG2DCE1IjIBjqkAVf00EHXCyiuukw%2FAT3BrbN%2BMYLa8SPLZ1CcuORe4cHr484x%2FQehBu2HT3OcZbADorAbwzthB6R3fvae4f3ggJW3JUAWuVlLg5QxGo09MGL04B%2BHCZe0Bzis412b5X6ZOu1upulG9SzVz8PAPoRKrspl9o5G7jjmoxUOmf3oI0gvFC5bQAnAY3tBmMlvbLoix47txaOvzZoqJE2SF7My4bBfOnXi&X-Amz-Signature=24d32844b9c5f5b64c720f0c3d2acd196be8a73a1778000651d397a3d135c538&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

