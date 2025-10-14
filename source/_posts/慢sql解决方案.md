---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YF45A4EE%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC73dMk%2BeQWblq4f9GCJCixbNPxt9h09%2BJ3ZQpz%2BunNoAiEAsnNY8vS%2BsxX4FReYziXv8%2F8KpA%2B9GsXGWM8Z4KK90Foq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDCQC6U7qHuuCTc0aoCrcA%2FCTleU87LzZxMGQn7pxR9dNN8r27ox1X9DPoQc4rBvpPysgJsqtH1AqWWgYrAc6XjpSPWTgn%2FUx3QKm%2BhxhMrcxNTW9EsyBsm4e%2BkUwjzCYph0Q0ibnWqcbCYZUw9KIQJqQgbsnzFuZG7%2BddcX3pg8OKdR5V5%2BXqyojhwO%2FXdsfoY%2FKw1mu8EM9%2BoE6i6l8kMgB8antS2b9BFlTVVAmoSuwzTYLEih14cWdjiJ2N99GtdBzeOs6mGlrVHV7ilmdPotRKDkiJ3%2F2DKaOqItf%2FAjXbU014hZUqXbZSkliLbiBU5o1jKFfuyVadRmJlSdpNvhoiRo4%2FyW6PbYRtKHctfcNW%2FrAq%2Bc4DtmdkCY%2Bi91y87tHzkBNNyVJ4%2FyzW5dD6vTAUPpEVUie1kd4vAO9Cn6ytftOe5By8XvXAmWQ15iMsVe6TAsK2G%2FLf%2FMTkGb0JbaSPAJLDjH2khh19TNIPOVTahh%2FcNJVRC%2FO4s7nT3Fgz6BaS9hEYa%2Bsv7jaabjB7%2F4lZ9Fz0CWzDTBeMWpNnVKPOdgVKyca5lF3H%2BU9MXTlXzdQfZg3hUnR%2Byxgm%2FRDeXntHLtcfvdKBiMpfkSrBU4ih9Rc9N2MTnjnxz99lAdOD22T%2BzUDcPFWG%2FSYMKiYuscGOqUBdVHJA1r%2BZDkNhdOiFdF84ipTCk22YK6VVi%2F4%2FZ4sIYGpc53dFZjL%2Fk2uyS9H7y874qwbMgrM6H7sHWZzs5R8TO6QnD6zvnzfu6uYhyMjwPfySHvyGFd4PuAMFgblyT%2BJ5%2BjnRBIjEmT2Xwjc54SS6AhPGg%2BLNEDsfggAXoiJE7Dev%2FHmmMlNlB%2FhPN%2FoFaBavv5eWBPtuj8%2F%2FDTs5iFcLRQMgvSl&X-Amz-Signature=c3d298f1bf04941a0841cea58d2c090fb0ccb0fdf339104c0f31961f1cd26ee7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

