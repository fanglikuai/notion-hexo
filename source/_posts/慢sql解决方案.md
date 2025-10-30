---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RWNSSNI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJIMEYCIQCbwOebCI9lDMbaOM%2BsxF%2BBCuswTvtaDME0rCiM9Sq6gQIhAOc%2FhVNMR6UR0YuCvDSae%2BM%2BQTGnhDGFHc26Jx1fNmdPKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXb5fNBvbQUAMxV2Aq3AOcU4o79PxMWXrB%2BLXO%2Fv5%2FEp2Xrg%2Bi8sBb45cgiyiQhZA9IyKL1GehSc6eD%2F644bR0s%2FveZyE9NbEKRyPfjC8Ou3mpXDx3nzFK66aWYOYbWU9Egm1xnzR3LF6Vsb%2Fh9ftdcEzsXj6Hr7P%2F9DlG9JNvsgQ%2FnHC9ZNBuiRTomNxcITChlFe7eIPDgEOHVi%2BfUsQh7VC1j8KIolaBdul%2F%2F%2FtF3W%2B5rM8yLAaFkqtZ9DP9XDKWOnYHZLFkok7QzJcCltFnEPnCuMuqBufd47saTMqIV3OiEZh4a1OX%2FSHZlZoeGtO8AArXkDVZ1d3unA38sGlp2%2F3FBNBsc3Lf6Y5N0akM3%2Fcb%2Fu1cV840DtLz3zoQ9Kf%2BCYRDEfRuz%2BBYp9R5whwRYILgnLUvfQM67RF2o0kTIju33zBca77UDJTaGgdw6X4TNR35n%2FW5LbtnqAwjbFkzzStQwNaG4gK2S0dsMQNHIZmnQoQ%2BOxxhDlaqinAp%2FpNxtI%2F0oQmslVOOPUWr1GEZp8zIfwHDY9KvsVakXIWDqY1rOmdc8wKAhd7C59X7I1rrO5Dhs%2FVu6LuuyOQASTr8H0EmTL%2FX87LuN3zp7Vro7NBAFvwdTzVKKkWHwsT3%2F2lQY25ZYrRNbylGfzCRlY3IBjqkAT05YUkTSOpimkjp0JErVn5UJz%2Bi23nP3WSKyDdkWobgYeS14BkY1CLf2BfyvWgRyC2YKjDAmjv4VeVJvMqtcgn3yVGfKKbpA%2Fn1R3YYMlqRrBCjnsL3fz5%2Fhi1zGOhjWVnMUYyiveUOQ7HSPk2%2BgbPJ9%2FeKEK8CvR9FUtfWfU9iJeKqV7ycYBs7wg3wKmU2QFlAtgA2hhRHePP3pgtlclVdgy9E&X-Amz-Signature=0c2a74adb7a42603f479dca91a6750ad1f08f7aa0bff186eea95c960b4212941&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

