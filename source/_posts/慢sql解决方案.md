---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3QK5TKV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIFuqSsBDCBc9%2FhWpUsaAsTD7XOuhCeu4RfG8WPwPmzGwAiEAgMdgWuGiOJV%2FSaKFwc3MdSpAUO8%2Bw3a7hwZIPaYGpncqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoYQMWGfWiA7IwWZyrcA0Ni00TGWRwdZwHk5T308SyIjGAbvmjBIYSK9MOrdZFTqnShhURdGpYFZ3HIsAe87U5kc%2BE9JHFF6Ld%2FPwSzToqDeM4h3bf7V26jLDpYpwgPaB2kiuX5%2Bhe2%2BMfpSoYZO0WjqjqQ5uozbmG5OLRn4s8zD%2FeUmxsPhd1mIbcxrJfsgi%2BicFJf6EqAS8YD1mJq28bG9wJkSj%2BdC4njPPwiK869UvqNrWy4l9PflZmMyCwQ%2BnXFZFXq5uOLdSgVvsG8qeZTpUIqQxwDZZtXnNJJDxpO%2BEL2poesaqUgoMRNDIhNptv8yh142vHwSyHQs98sc7XwzsN5P1DH4za1t03QvBSofeVC7AOcy7EzccUV0WVEBVjlIrD282yuV81n%2BAbgK6K2H96lLRtPjL%2ByPz%2BrkrkMkwAwKOuLf5ceGL2Pjynw14hPWZD4KqLUw%2BQINqD%2FScY3tkN28NQusditW6rp%2BMfQ2SB6Q8yefCJSH7GSuwSQiPSE%2BCHrIViHxjVmw9hngPZYrUhqttq4c%2FH0FATW%2F%2F6XLlwj2nf7LAXB5G5D79HcXP9nZsGG3ZRzTOclEZxgfx2YvGuukmaJPq8YunzDoJxGqqb3Yi%2BS9MA0r1NMW6oXMH2aXsf6XF1DtkTaMLWc2scGOqUB4585T4lvUwqqqHThQ6S1MxyWyagbRZ940jU9q6TWePswPZURJD6IgL5nDEmkBgcKmgTwcw2KA5PLbTfPtLPUyA3Q5TWHJMlXoJx%2F%2BSulPXirEsBodK%2BMzf1%2Bl%2Bg9I1eoN0SE48UVez%2FQgb%2ByDYYKw2Jlks103QfxVY0jU8fjpHD10ilUedW2M7VBfF4S3EFqz8KrVhfUJGYw9xOHV4kF9Adz7muG&X-Amz-Signature=70827618cad38aedcd69f8e2e4296f25617b9f96d1f552aa9ef930ec43ec9a94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

