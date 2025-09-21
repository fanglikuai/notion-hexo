---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V4WOIOH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhgQAoiua6WGU3Y%2FN%2F1DMsB2O4fM9mfWzZ50ZZ29EQ%2BAIgNsogi0%2BfGjpWE1R1WAQzP3l%2F3xu7QyjjRRfnhzXwEBgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxqFbei8KYrNhG6YCrcA3FAGtNDQRWFUsGkBW3XSTz5zKSwhZ14mghMKhCcwcLfx9YOBzf49poHIqwtDXvZqVCKWR%2Bkkia5IZBUIH%2FjghR7onJi%2BSmqVU7zrow9f257R3Mj5TyTwyms2wQlovu3iO3HNUN49LluYSVim8AIr1QQov68maxWcEAA6%2FD5Z0UBeLHYwra17%2FcAbjEx02lCF1qbVUl2co2qvqIJkcJPJZsk0Ki5x7DyUpUrU7NqpvHNfTkpFkeVB8seTn6Ds88TukW4pZLM0y%2BF0ZPfMDma%2BvUiI4WN4bkhqtYiteKOSGcnEhyMexRWZwHmC26JeL8Ohol4fQrOLWHrMWGL6XB03O2aBHAFUeeW0N9C6EnFi8WuYs83FdIe517XHVBpuxU1LnVCdIlbMcJI%2F%2BDCKKptOqnv34yoo4dv3CCz0bPaVvLSfYZn01By2ldE1PdnE2djim2DzIGKrmrnd3zD5KjP2wKp15Ip6gqaoCR4PpYRuJmnJeLVdOkXKvWYr70jTLaneYGYhRFl9vrJnUpNfivYpKm42ZsneLCrePT%2F8Q%2B3OBWjepoEziKs6qSbOe6I%2BP0FnP5%2BXsC3X4%2B9yadRUIfJOOYaG5bUCKK793TDR7lf1u6osTpSrUYLLGScEMmqMJSLvcYGOqUB3a0hvWwx4N5helFzXZNsMoQ8zBZnc71B7zjsNUTEJI%2FJhP40DxqTls5OYx45I94laDIAi5XIZvBGh6XH7IkYUraqyV497JuXplQ6r%2BcCr%2FVeiB%2FU4PauXQzowRtaebgTHAaLYQyrj6vK%2BLuXPf6D5LV5ZKZ8nuA3kxbCoImRsaBlVG5M8zOgvvxsnR%2FK%2FM8htuCQ%2FZ2L7hQ5N%2FgmOrNfHJaSDMiA&X-Amz-Signature=8ff0986b3081552b6c4cfdb5451e38d023c5f1f9b06f5f3bec1b25d63d458bc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

