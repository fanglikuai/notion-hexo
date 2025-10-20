---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJS22WSP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T230052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCRakER4Z7M4MhdsfwPr%2FfcoqeOjruEU%2FfqDMJ3r34OHgIgBLwwAgmn6hxPgC8SAvyzkqEMl2yXUcHuA%2B6spnESVNAqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMOvtZgwARLIBkvqZCrcA7htZn0Epp1r5JpDXGgvQpcoooRLsU%2BYHsdvNwX6Obi3WVJM%2BYtxp8EihHrJBe4psoo8uBPnWieW3lb%2B78EU2mDgUwuD%2Fv0M2DshBkeTbeOkG7QVwa9HtYbh9JmHdSozA5VhghdKg3rj%2FfBWYJN6XQqgK6dAJ9TEO7y4ockD6XIuw%2BGkKyx3iZS0IFj93V%2F8ZKF98iJqVbptWPIicsLlXPcKs592UcW%2FLPhRFRq2jJe0SmxgE%2FXhyOTAq5%2B3FmigpnUDv2afWz6wpRIWN%2BXhY%2BP9iApkZeNwfMRvDgVtTjQqyG3MpsG1MPohTS6fldjx5Hn3RKaqhN7fPDbgGzn904KHXSubjzbHnquVcPVyy4jlKWs2sGp18l3AsuS0tc1tUhreikzwknv3yW1lx1T17mv5WKy102DbtySyO3NaKWGX6TQQtIjllXAiptYfk694pTXALjSkNcNtsLVo4S6mjA9BKFi3v0i6aZTHvKHTZt1%2BQYld9tnCxr%2B%2B8x2%2Flig%2Fyl3axb60iaNkg2GvJAehp09%2BMQ0tGxaO3UEFJGCjkIDppyigLyAlnjpRq90UDv9ETEpK8lGenEK2Wyphn%2B%2BNsp0Dq8wMS4rbxDYHkZ4Jp4GpAo9nMKIINpeWYeXkMPno2scGOqUBQgxwvl2rbOzirULFmrHTZE76rT1eEvfSnejYoPZ6YosaON%2Fl4kQF2rD3ui4qwbbLYFRVcRAOB4mGUGUoww5%2BKVuG%2BKcr6qhprk8aNyqeVkimFs3TfqaKG%2FeTxRhwzP0vg3sgdmPRHzypkPQZ5ZYD6s2Gpr5%2BKz6LSFsqUoLuoRZiRD%2BFRho3EcGr2ugRInce12gpikubq4x5qVTQJfuD5kXsdiMv&X-Amz-Signature=881bb800176777f843f8b990ce6ccdfcabdc319700c0923be82c40f338d70e78&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

