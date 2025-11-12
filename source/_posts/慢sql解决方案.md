---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672OJIPD%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQC%2FK%2By3%2Bo%2B%2BriMcDR5GKTKidA1AHt7jgccz%2BKOusAmHIQIgaDjt9sWev25KDjuUUzr%2FHc0lNV6TnDe1PWzJATc3aPAq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDJvsSMGPvu4GpHsFlircA2BRu8ix%2B78ggbOZhZbVnNGsIrjv2TH%2FFJ%2B7XlkpAV3fJfVNmBizhuWaLok6COPlZxk%2Bi5B0CjMqMVKFc4IK7fzqBghcOI7MKFEj%2Bf9n2HA6H3jPjgpmxa2v3uioMs9Wsfg375fIAC2EwFhabjP9BZsjsitXu%2FuR%2BuCOkQzE4YqU6UzbGi5x2dTSnoBKZImhYm%2FhhYNUn%2BysTRlpPC92o8zkqtCBuK5%2F%2BF7BONG4XFFK7GFhArY2%2Fb0lHViMKSFfeUoed4NWEBI2MiBT%2B333jHrB5F8rmY7gbMbca8AUpv7r%2FJJaJZuDDXENbi3myeA8UAB8OO3FZ5KxiakSTJOw%2BGeoRQ93pRKhKXt7h5rJr4Ob4vLHdgZ7KU15d4k7lMNgQDfSD6vGpmT7x%2FL3%2Byh3mevCVjOFjGpN2y%2F2RmYxDPTeFsj1XSjXzDPDdBbjO8xFmKxkFsJZqge6YAc%2FfPZOJC2mP8aY%2FojWO2P4id8Dob18D0p1vLus10EBF2npWbopCFI%2BD7nIxef3VXnq1iYvSnqIQp5P%2BPKxuh%2Fzn1XcOR4O6LZ%2FpSdMQ1rlGikynDEYkz3IgM2b7bHhDKLCP%2BBRYSx9Ql8Wppb39etFfCNwRFegvUI2AQ4Fe65fvnUaMKub08gGOqUBcJ5XpxDyvSGE%2BgXNNh2U4EJo42WhfEym3OJD20IdxjCLLEOUEvaZnBV2lrdBsBfIDmYyR9y9rNsqKtdA3IySH82ytOfnHWqCus%2Fy%2F708JTrtirIJuGSp1p92VHEP7p1SyrnHIudOFlzsqxo4hihtMywcFLo8%2FB0e80BroAbYAhxhQy2n0hW%2BHFJt72uytheFEInauX1Q5YpbFM%2FxGA4y4qu1bGz3&X-Amz-Signature=161967644b35a6c253f87b5db849bffd8dfc48a621d9dee3d352fe0a3a50a688&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

