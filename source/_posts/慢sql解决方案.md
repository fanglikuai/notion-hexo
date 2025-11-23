---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFLCBMRT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCGsUeCXl4%2FAm3uEZef0xFWzp9Z4Z2jTqOMAjrViVRlZgIgQ%2Bu0LNhVYKSnxiDcS1IQR0v%2BZX7Sb7uOZ%2F79CCFVF5Yq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDMhUtp3iCNtsxdUJ%2BircAydM4ozFaywhs%2FkObDjm5T3%2FK7oZM%2FibX6r94arFs6pNgOI40zu5%2F2%2F1aK4geE6rrBE0o3GMZRJFgz9SN5Xjck44C%2Boym4EfRbIzxoW5TPBtlvHtfJcqDW0mjIdmBookr1Xli72n%2B5wVgMRJtNG%2Ba1qRS5VjCF7DCk0MH206Bo%2BfunAS1GwasSMLnjxNmfBwC1z2ERWKbjyStv0%2BHLHrdjFuXFVR83Mh8eFo1u3hK8HjuCjU%2FxE8ErhUvk49VCAuyHTC9hhUciuMTWdfQpguRaEF1GT4%2F%2BTK5beiLJ4sRkZVCOuKb6gKtgOYcV49ocB7tdG%2BHk8NZ5le7kpEp3pqFXP1AcUh5%2Fx0HWemIXl5c7N3RXCVvZeoxIRX0vtq2%2BUyoxdKvI6SYbx86lE2dnLAcQBHRnlLB0v95qUGuDrVEaqEH%2BLxYeZn2X%2BbwI6%2BWTAOEoGt71kGzjmh1u%2Fq2MwHX%2FluColN2SuYfnb5nKTXmohQdMAEUvf8TPcRMihZP0AS5yZgZ7wXR3XIc6X9fP%2FkLLE2TPiD6qMWLEOIAMoqhSQ%2B%2BpcC%2Bi%2FOdMdKCAROXoSMMgOf9lDy4IKd7E%2F4GtQVU9R4VPd5w4xZAlSBNWjJBF8rCnXDOte97Zn5KtxIMNaXi8kGOqUBQgeIdgbEgm02TY7FmJh0cGbyPvdvtBS%2Fj1J59mfIWBK5Hmu4N3UiXhevw%2F27iKVblisDtR6zGLc0AXZ0nt7ERfOLPzZQOi1J%2B7T%2FyZdN%2BX0Pj4V5KKlZLRsvhJOHQGPOdiRBcUMBcL2UmVwcYRh%2B8lWo6zE4SRDCZpysuRZHMnYnQqy4XA52%2BrdfaQqrJ%2FaJr2yuhltU9bxpqLmjbfpSoF6kBoH0&X-Amz-Signature=aa453685d3603d801e48c4928245c9006497ab2db55dbd750a156fc9a8b9a9a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

