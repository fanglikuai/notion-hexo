---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2W3XRD4%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQCrY%2Faf6nKePgTMMWBEoYICWYAPKKuLEVEunVucmvOK%2FQIgaLzFqSRzo0IJg3nLWlJkZET4G4AM35MaapjEZbakoxoqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFl9GPUQam7rIlAEHSrcAxSZv1Veaj%2FQd4ZGwj1VvZn%2FWT92FG8KvLVP2fTN27BTlcBulMC8X7a1XMlGKBkcZx4bhU7qkgfSy37VC4THiatQaz5a9HL5N%2FwuOay0qHjvfDABBCMoKNbDPtWeatCSB1JrLl4BJJxvaK9Mx1i9Ig6nF1oFy1H%2B%2BvgG5UEm%2BlFCKVpgPNYwOF4EpSh4ibxM05OQQ2n6P62rh6fCrwvD7sTxq%2FeHLweyFZznbm1An9B38p6ZQFZssLqQeCQzuNXZwCLV%2B5ClFZ%2FX49SvroXE1VZ0y3SGnohWtHdWV4jea7aly7MUu%2BY%2BZB85A7wcuiMqkpNAUNsgiHUzvuxhNinRZix7rP0sGHy82UCmXtvNkbSc6HlOKf8zI0KXqwhQAkbXuHG34vPcKK92QkFNC4YxeILh3hwrUByAJQ3fo8DIUGMJ8Ky3PteLK7XYklcGkB4gGW74Np%2FIJZJSGWpZY45lXsuKhch0qSCgBM7JJemVfvo88Ljnq3ngRbdCvGki4a1tCHvoZnE67J3UAkB35bQeEg%2BvfZt9Ke0hQTj5q%2FRwUxRqKc9Z3Lo8X9NkynjNwWmUBYCPWR1cgpZ%2Ba4VKibl9ewAiBe9LXRsZtcNWdw5%2Bj6ce%2BGkKMcSgY42Igj99MLHqucYGOqUBillWEu6%2F1Eyn4hCd84yL0yz%2Fg3CnebXFAWjDrhCRrMwXcfm%2BmNw1FQYK2%2B0W7GeoedhXf8eWrVHm9frB2s8N8uNKw2KkMh3bTN13vfu7ecmhLdedOFFi2NLnvW4I8Lf8yx5QMxmwpET79u%2FjvmAD6GjW9DkpBtrrHE9Y%2BJNI6%2FjkGlnZ1rMY4PDY%2B2ypSK9NhA7DRR%2FSfVuC1jt6C3Cl4I1U7RR9&X-Amz-Signature=5972e2bec3c96ab10d5accd6b67bf447703e248cc4ba1501fafd6ac7880e13dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

