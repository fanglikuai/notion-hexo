---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQKZFBQI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T100037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIH4iK70Vkykl83iETkgdZ4uXC%2B5FUYryGgeCrljAmVOmAiEA8BtOKrt9CgwveOefzbteHajwiKVDb%2FMEDlRtJBq%2FEFQqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKCr7d%2B2p6Fo%2FFPc%2FCrcA9KTR%2B8KtS0uJuEGDOXE%2FV5Fn0lIcmsaO9d6RHLbFffx0GOMM4MqnfxK%2BrHyg37KZBoy5ro3ZyK%2BARqam%2BSIb%2FrhDcdMAfYoRt9edQ3nhO2hGZQbC2JIkKe7QspGpMoJEU5wY3SMjGU0EZgLKOOtUC3lnjovTTs3cJIN0kOnrMv1uitEDsAjxED8gxpcOqUdf6U%2FILVAds3bOh5EcmBkyy5BWPA7yU4jJy5CPjXh10w9zJ8BZPj4dHio73fd%2BRupgxETcxMR%2BXVt9NyMgBWN2xeo7Rm4HwVxKMr5EhXnrPUG4g25xxrm7bkYmOWAnQNR8OYTbHYFPsED979%2FoYoinQELTFNwfgY6aS2cDbUVFPeSeEc6OGGJ3DyivtLqOYwdhEoJEk3lIlv3HAFrJeaA1wc4OjABzzuH1IMDG8sS9wOzXF16rBWtxdTAktoQtHnrAE2do8NvaUEO8KYJGdFDcjmmLkSMCRD7NNrHyhRdO2RO%2BzgClpKsnPqQYrh6x4I8CwCfebXgDQ%2B4D8xARPt9wOBTLuwg2vFGFyMb1UosY%2Bm4TwXLAGjrTF6xDBRsFKCLfBz0GhlgvgO3sat68%2FbQART%2FpQ0lXwdvTiHWoe1kXC2T4Sozzj4COC%2BkpOWPMLa648YGOqUBJQD7wVeIQ2hmKjddm5KIqyf2v4JXkZ8%2BbfB7nqdhH5Z%2B0n5w0gV3ku%2BUNOzebFuIY0veapNQPzK6sV7TFQz0IX5ThB%2B1BOawnm%2BL6TdS7eD0uCCxy2orUDZSKIzX7ewROwOBEotHAMXvtE6Wx0CwFtG%2BZdKst1QdVGXAD8d4%2FblLxxE3BDAPqhnHuAzhR1Z1AIwH%2Fsj7C%2BPTjEQnNn78cm85rPU%2B&X-Amz-Signature=7f5b3dacacd10fea9eb551c2630e276b5b7393b4afb907ad3de18965e6a64a3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

