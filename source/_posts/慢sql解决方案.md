---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCH3E7JM%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICM7lQX%2BMFSmCcH3rG9kZZnpc3dd8jKD3U9ZVBBpJv%2B%2BAiEA0Sh9jOo0fjGrPW%2FIkF3mFZFwFvCAeUFLrbR779dWIpsqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKWCv%2F1GG0OTszzYircA%2Byy1rBjqlH6meOkv6HOECpGbVRygBUWU%2BUpIfVn9WjQioMVRo2r0YabYfmO7ByUgKKEj7nH5q01u8bAIZzbu5wG1hJCzVGBzPPQI6bvnv4gfRAp2RjlQWZ55t%2FkrDsLB4Fw%2Bp5CMrUx98zOZ5MvYcnvpxCCT9k%2BUvisTVsH%2F5k0o7PNe0JicRRG%2FKOVWV%2F%2FRAAY4ldy6m88OqPN2HI85GtBBpziM1ahdYa5cJlpfC4om2ZmHNwHkWYFupk0W3ziQRye16GhAOkGUP43xuiUWuctyJt4phckYNFa3zCo7oFDt2JMQ88cK%2Bh1GJcO62zEgDYigDwOhicGxDDmcbdPSV7DB3YJ0K%2FmRNXWYbJ1lQ1qqTr4UX%2BdsJ%2F8mYgMKx5gjq25u4ABJZYij74RfMYy14a1q5rbSvHjKlcWEaUvRd5cJRL%2F2iwLEbayG2ZxMx5%2Fi1BeLyMpG8wminlHvblSLt19WV%2FDqTNBAr%2Bthr%2FN4i94rqpToL3hbaKi3EP1lYP0gx9O2g5n%2Bil65mIEAK0HCx9GxBKNQ1zNHy%2F0uoSJHsPNsO7oty4m2WyHUIKtGF%2ByJuol7BethW9ZoRai5oOzYSDihpxPnuc7WjWXF5X2NWp9vf58%2B5cKIwbxtuo8MJXKm8kGOqUBQP82l4M8URZ83ze3Hk5%2BBUDRINGv1muymzq6tDZ%2F6fCehtdWWxscAwuppTyGUBU4WWjdiz7mOrIkElYXeBqQmHKjNTc2fCW7nCkUeNZEUxLhB3c324EfpVwXZYiV%2Bm8IO6JLBycJUl%2Bu8rFtUV0J4aBTwMTS4yBnDsUvOIL0ZIGIgl4UFA3YNqujKm%2BjDcbAAO5ANppR3nwmFZFKYR3DAjcPYI81&X-Amz-Signature=62638776fa4bf91c1e519a89ad93aa76bb50897bbb4236b022a1e387981ff06e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

