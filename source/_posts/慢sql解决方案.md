---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3RN7SFJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID%2Fh2s6UhLiZqFhxsc1X8nlBqD54jBZ0oroch2w3ZE%2BrAiEAq0DZCF8Mw0RxogtAwCTuBglfp%2FRGdsM%2BlogGsXa1mjcq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDDTIfoUnflaao2F0zSrcA9WnP3JkkFAJpXv4xkSrsotMB4PJ5X%2BLHdRgp14HiwIKbEJWyBTMWZrNW3qUtH0dU%2FYiIUPcVdC6acCd%2FWSjJknwx41tLBHHrrDnG9hciR8kZd6ghVMHRYXPOMdl%2By1x1jIZXFce5ZMGNUQ0sqHslDmA59xyQX3RK1ux6HX1eRocgSceGLYBfrdGAGk8YF5RqQWSQyyCfXZVZl3XpBnz3T3hLa%2FOHpEGIkPZEPCUqhAEfZJymZZA4Mr%2FzIHvU9tZQUyBdrsOJAT%2Fx0tZ7E%2FNjWmPoI9FFoGL92hxynXVGzD1qc7Cy20p16%2FW5j%2FwS52f%2FGzFlD2O733aou9a4GJ1wiBId%2BYVa%2FiDkdhBoaDGm0%2BpLB%2B9KckaVXxx1VYgMWW77vUDs1G2vRZ0vtS7Ck4kYJBj4odSZFTowcRRVe8a9TStBh2qvThd2aP8Z7nfgcJSi6pWPWb9Pb%2BMgYgtldBgEJ0lk2l7qB8Mejjhn1EAnm5o2W2rTZR2JD2eNvR5vLOVh3tWSyfPyIihJmRQu%2FwtlhHdLADbVcuYJKu8iaNd4BKQuWlVnh6ZmJQRn03s6mS2R%2FmBbAIJhM06A%2F4JUyOxTfJ1JIL6fZSRNUANzr3E5UXWUE5L%2FM3D6SD5sieLMOCw2MgGOqUBQC6y66u%2BCehS%2FfjUR9RQ%2FHEefi7CdFLcZwTeXhXefs7tptoolpA4v7iDiY6lR%2BBXmGMpGnk%2B3mE3K6uVYMlxV44UOFaCYxRK2a2gKl3sXR8uQ8OeJczhs7RGVFhE1WZbs%2BgucHaXr7JNoE%2FpNjugBV6tG%2B9Wn9MkaOSyj0p%2FjygN%2FehnN9zTArlv8jaR7BnitrEiqNjQ4y3wtUAWcH2hLSR7jTxS&X-Amz-Signature=d67ce38fe3dc754d4deace9ef774c8b54598c8eb7667d6aa57f8ffd629f2726f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

