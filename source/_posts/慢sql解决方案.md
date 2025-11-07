---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667G3LOTV4%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdSNp6mMGKs%2FgKue7xUi02cizSD7Ftg%2FszGEzQXhAM7AIgTRI4Ueu0FiPm5jUzf8d9fJq5G0lK%2BqhwKyYUrAuhCc4qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIgr1R1%2FFYQtlc3x9CrcAx3xeObIzE9E4e906bkfJD2fAnw70Wq7bqel9owwXsFP5%2FUGZ9eEkfwWQ3cKdFHOsRjiNL4dOvOx8Jg5Ci5Pn5DiHmnTuroC%2FrGnqguz1USt565ctbRBxIZ4nYI%2FMVAsXS3lpHh2v9EZKPT0aer%2BPlvYznSSubFxvNF5L7aZHH%2BQgxDlOv8vsPAT946MTtgfk0rjxW0GGIxOVj%2BlfNk2USbT6rctZ8OTuLLyuFA6h0CfnTSefSsSNX8lJ%2BFngqPTKI17VNFeQP4t6H9igKMw%2F%2B2RgonVrErElz9IDQSmD%2F2BhFdI8WTNeLtSs9LQ4zeRQunZLUqMK%2BpxAo%2FlAjRNz%2Bd9IctyyQYS6Tkl9kMS1FOq5ICZ%2FBXLjLZrEgldKRPqBbcExNDno%2B6KDCvmYyBBdPxBUyhOqbz6XqQ0U2dQGt88BmCDjRy4as6ZpSNqZrBFyz9spTDCyVyRhYjgqUgKrTO8R%2B08Ky9NO1JxX09zIosTz2a7JxJ%2FfxP7HdxgCQmDfU1cUkkhdPIziXKgSsPTiMc%2BKJ%2F5YvTc3BBSNlQywuGVfgRU6H9Qkn0qQHHma3j4yepyGGp%2BLpQ89lpP9i%2FDRXVtonLrGqnsEZNzY5Mk96jXcL4yJpu1OZf%2FH9MeMKvguMgGOqUBqSA9G3qwcj3yUpH%2B5eNXQRiEhmkvj3dntBUaP7cFAeYwLwnG2gPWJn2DknqkpsHP7vidRvlPpt3CcYYGnip1uuM3GCpNmjfHTmfksRPzQ0HgbHA1Xd5eZqZdhEI%2BrcfWxQHX3y%2BR33L1XK%2FU5T3ocvGmJLbrFiPt4oiIhiV4qjj5B2VMDqS6R8Cc3cYpMMtKraCswZiTVtOtSF72qQoivJyhh3kO&X-Amz-Signature=2ccd10c92a96217dfe247146a9151ea983ef50b5798daa21ea687ac26a727768&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

