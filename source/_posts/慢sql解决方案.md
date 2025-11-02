---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666E7XIEOA%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCICTVI2dMLU4f%2BYJtweOchyGEQKXIwy%2FiZ8VrbwLV5qc%2BAiEA6d7nNJ4ZZ562XViN8nc4nCUtR2PPp%2B%2Fr%2FH1izc3K014q%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDAO4IpNljg6%2BwEONHSrcA%2FWskkJ%2F1bFPdHNGfMu0RGNOyeRGcN9GxUHudg4uUNDjQrlDp%2FPqR5jFvpdDfyVAeB1xmS4QfTf3ekb7bsgMvyAo5G5yYN7TGmQqw9rj%2FDmo0rTv8xmoG2b53%2F5MyhGr7cUfVuGnpL3i62k2F9puGgIGYvYkLlesKu%2FkZthH2f00Sus8%2BHhd6zViTibjQjx%2FFH%2B8iIMgWOQgLhFgPsAcbliPI2GUuyi0zxE1uMMHII6bwCTjM9IdmHw0PCpUGIiGUXYnAiqLTpq5dAJJUCidhObKv6cqTLeQv9H2Mn00amkmWhvTbKGkK8ENWPRBBmS8yikTIQx96lEp4u1vL7infm8NOJLH4HNdBKbg4k6I0RecT5llVFg%2BE2e2Qz8c%2Fx86GQgMLEJcSi0Ev%2FZZp2CvkWeNqLile6yZKuYgEV%2BtJRwt3Hexip7XTPIYoR8A4M%2B8QhPlX8QAgkWAuV1Uw%2FudTeTvzMl%2BEM8NlWDeqwSt0X9%2BnCcuXP7QP%2BYDTUVGoAwuPnAI%2BkVR0JsITpGD8a7NNgu7XW97xSO%2FF6z0sPeHtt3KtCknnltJT%2FAWLWRLsrrzBCjPnxYEi58DyQ3DKMFTy5AvG4IV7h1aJurN5Phvgki8of2dL2T82fl5ejqpMPPincgGOqUB16GHPCzfz1Do212cZuldjICxhT5%2FqJ1aVq90cR%2BPRdNrwrFRDioFfY2Y%2FSfebqApjXckVsYHfkBBwY7AjIWMfqJ2I%2BUb37WT0ko2UbZlDWVl40KbzjKWQPhKyjfYiypb2Oh9aGmLzVZcfkJw%2B7cMkFhCuiq38%2FohoPdtKAOZBqFXCrbDNVbiCTV9JdX%2FP82QSxImdMWbGtSs4fO%2F8YoQMN2Gmxut&X-Amz-Signature=9429fda9291326cbecd12cca3c51fbd5733695851d6f937ff3184d3149fc856e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

