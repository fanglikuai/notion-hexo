---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2A6VIL2%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEOT%2F3DFFVPfehQjbehqAImy3lXA96%2BIg%2Fo5v809fT6RAiBxST1Qc%2FYfrJtPU1ZVnm54AapD1L4cO39Nc%2BXC1EsRiir%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMJDOqndOVNNXTBZYKKtwDuteArNyQp1HqY6%2BVziO6l%2Bu725DqQcfW00GOHfNdviqmOmb9ItkzNNipelPRHwWBCyLi1P8jOMgaLw%2B0Dt4MRKJRtPFV7oDynuDV5E0qQAdT0sBgNNRmP%2FkpkNnoJKCCbvHpW0cotnk4DNLeFA52HOBh64IrBb%2FnM8GE%2F19gK%2BMVByIh6TvQ1A%2F6o0DLH7oE%2BUTt10TxJJOEijsaQ38hNsB%2B%2FKkbZD88M3HeVmetodLClEVuByuMYbgo0wwK%2FuFMxyl7wJpLfwE8WG4peCPKL7JewE1WHwhwZUmQf8%2FxLtmGWMBkY2L0HN%2FmjD%2BoomHyxyakyXSrOiTefjtGW%2B9Sr61tRJlHE%2F%2F4Gtxdb9A5%2FY7M7EwxG4%2Bc5mRKf8MbIVXyF7aN1VqiOLonoEnA%2B5HvmfkJlSTwfmfvoK5ie3A%2FFEVbL7rUfEtrQDw0onJE37shqCCxs61gw450l%2BjnrVTviZiSPxaOEcc%2Bpb3nbCusG7qiNCtXk4ne6zMFEZEUKvD1awLKwN8AlhfzuuOJKfcgaP8XvycmGDsXuYJMBdGCO7UpiajjDCmHjNbQDFZuu9BQnmm%2FXtBfpturGr8KWSuOFaNhW9Va65MzVegciOOJGEp2MyvXocgaSTG9liIw97STyQY6pgE9YIuETKfi3o7WeCMDDJWv0RR42%2BITV7tOmhlkPF8X0d9YhgmVFgwd%2FYaRfdS28i3mF84zMf6nMY0dVV9SClncskBYudi2EpmqcTDK3Fnop3LVgcrS7Lev2v%2FRmbLPpTHc2Pze5x%2FMn9zNXyYzgLBnBBugXSbIOA%2BcParIWVB399EX5NU2gpXw827YOSlzuW%2FYcMil3ZZ6LIm6wg8dk59Bzj86nWU3&X-Amz-Signature=6f14286ce04b63feffec2e561662edf92621cac3cc2a0268b31c4d2f74092981&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

