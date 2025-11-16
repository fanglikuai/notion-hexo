---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFF7J42H%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHht%2Bb1aHsnqFKS6s6KdezAinsFJJ6h0H8o%2B%2BcNLCae5AiEAiIuqsX5UNSMIQtuHXpzOfZh3CLpHjuyMENMZnPeQcJ8qiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDJoFZf%2FbrVBJcmNISrcA7BDoahCiwB3Y03pLR1lJCms36z74JnyWpzNcxPWc%2FuMYNzY%2BoAQw5MxY98KWRvKyWyKFpyz%2Fg4jkyncTCZUKwI0J79Qjs%2Fmg2LbsshgILNxbb14npYfhh5UzeJrhQO6KXI7v4vBuKTf2VGQ1Rp%2Fo2jlyo7eHYGUmypldnGOfezfDhq%2BJ5ioP%2Fomd%2BtYmeazPNG54GnC2i2SWV73N6HT9XrIBJUCip4xiiL0sHKBE03MeaT2mYos8NOHfeACOMnt89ckWR%2FgLXNKW5EWZ%2FDUwAg3OUNmKORYU5xQtJmX4Ro98uerTR1PvXdATjxB%2FZrKlHweeCAEE64q9VGB0YXBRG2BKJWiWP9ZWU6HavfIaxNQgCcsmfOlMrSDd1j%2B%2BNozjDh%2BiX3TZOcbRqVKM%2F57jBnFqhEWtVngIzReuvsb0Vjbhu8bmkGn3u7GzsQxugL%2FUUFJO2%2BdRjOCqU1lTyIwVSgu74lOCk5dWtqtxF5%2BcbVbE2VTmgxnLIkCRSJQSbSI0QieCHqPCThK0TOLJ1%2BwbJrXTYt3LEF%2FmbKaMHF%2BCpIgcO4lLB493i2C7iwuzWrTDWRh9EDLVe4V7aP05tALt1INxgXcJGZlpBzpWZAGl2dhB3Jppivy2vrEIUQ8MMT95cgGOqUBs8cqe6C%2FS4I4MKXcSvEbyWRAmR32nwo%2BA1KtJeme7XwMAlPThHSYgfOoHJFJY5%2BgYoiF%2BhO35%2FA9An2J2OJO8Qjrj0gZXo36Yw%2ByYrv7UU2Kk4sRmMcI%2FVeeE1c4olr18k6yy78gjYyGekHjXKEyH71UhjKj263Il6Hnva3RIW7KYkgXsQFKe0iQNO5oEmFz1favomFVypNp%2BbWd8BOI3AAL63ua&X-Amz-Signature=195665813b85d6f53cbf8129373014bbe4bb4743cdffb9a5934ebe3c033091ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

