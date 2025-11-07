---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS5JKU7D%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T080101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCT78bgqLO0YywnyagS1S7xZhQGQvg%2BR1eQcch8vAbnCAIhAI4cxxnXlBdYoz%2FHUknIJHh3ueXzAjsPYF3%2Fp3monT5dKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3P8w4CmLcWxBXbXAq3AP9gVMzZj8J0dGmWcgBzpXy4nDR%2FbrH1c0pVYDz3dP4cfesnJ8Ucd%2BaAmPMtMjx0fpwVYKfTp5gtYkwIgfxjI5qeIppS1aNid1yCKTFWsxTUBkQJRSS1FslF6gOGvbZb4z%2FSzgyeaeebQBfDoobqRzYedJT%2FzNGdOLYx1EhjxKk%2BfAsqQ4imrdjE8%2B8QYnvJY1CMfyX9aYIfPdVlq2OzJzPRARX%2B5XfjwGC5B2TofwYjUPC4vsb420wnF%2F%2Biwrnuq%2Bz25wSPXJj0NbvJPl4dR1zZKkjQiZ%2BzGWe8lp1zoL%2FTHZyfUBe3Fsqk%2FC5uUfd69EdXor4bwKq%2FnJkSBdFeFT8cvBPhGqlYSTgX66Cn1GxZMItzJW31vbQikQB1U5ytjgyGgzj1gAFpe%2B3PrCacCTh0iwjq7lqW88M20kosGteiKe2cFCI%2F%2FuMalKJJT4OuatvAAj3TIQfShcwy8I2SayI1VVzSQDml8AjJgGCYsAvdR%2F1pEh9uGznO9tCymC2JyEPOziWBBH4JF6yhQCT5%2Fp8LvgFTFr1kvc3gvlGPtitj9phwhRUWQ%2BPxfmpHGbqmaj1v0LhdjSTfFrAiGJTQmNIGBHpDrWXjdKCuP1I1Klnt19ePG5WKurEpQU3QjC%2B%2B7XIBjqkAbaGMg%2FYU11lgEkW7ADjmZgNTDX7k3WNVK9aY5vEwdpRJI%2By7DUX1gMgvpbacIQ0vkv7g3fdUb8yd2cci9hCm0O6ElkOPHC3AKOhFdV99dfBQnpZyzLKNhT%2FtGHZlHvQnqwrad1YT5Io%2FVztfdS1VjSkTKXRRuixC2ZHthsQrp91w%2BkUTt6OJGHyJXlGuMfZvVi2ew5msigFWUKI0hpHLoe3k3WF&X-Amz-Signature=e30b098c642206d24b894fa5a425752f7038481860758f1e12d0cfe9e53c1f29&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

