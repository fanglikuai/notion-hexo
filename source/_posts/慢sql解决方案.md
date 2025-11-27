---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSZ4NVRM%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7MKLQowdzFWKvDEt5v9KrIt3IrbNkzctTRsZ74MWOgAiEA8SMJf4VAbTgbVNIpBOOTjayNLCq%2F4UIv6pJnGgcIsp4qiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPil8G0nqj7lcqaNbircA4AkEiEfrIJf8IcXogxpda2VxTR74ZA08EJeXNijUtP3zTF3t%2Fu6tlTTUF9Lcf4A7Peg0zkwcnEfXIblHmXYw%2B40eD5Gj6u38lUDnxezyM8bEx0%2Bj7qwY7xRTRuqtvdWrqR%2BK1UA%2FxdgIZeObrCXud1oyYNTuWJCtpkkzGGVCmWg6%2FZcrt78JaTVcQ%2F0FKHmiLkxds0lwfdWBDElA7f2HfYaFdmQUV6O7wPAz2wEQcYGddic99EmEwJLPU5gIjOR%2FSHMiT5LMJLw40aLHWiatOFiIa%2BDg60EkoXK0PHjRsN8InwY8drABqjPWzO7rAu7QFxU5%2FALotlYQ7tXO2OMtN7J84FQ75w2MpW7rdbsMxQqlH4xT2B%2FezU9acrBII%2FsiB9nn3dp6SGp2n3Cyf0q0GZ%2BKr7Smi5B%2F8sV0acgBPYeKn29Ws%2FSNKp5u2HQsal8IfGr5ay1fETxBK5COduD2mk6p7n10ru9KYM2JgnVAh8cf7LXUDpIfZ2Bu5qvNJ2Wy7YH%2FdtaRfQ5o2nk6ah0n0lMDvEz48zP1Go%2FbOciViek6%2FbccmPDiBiD7onS4BDpcqGUOhK3k2pgYDRUsx%2BAbe%2FMHPKIQNobiiFYUSvhVX%2B44PTX6fiKGukR8PxkMNihockGOqUB3iWlpKSxMz2ny5NuuHzpL0V4iDjJvBjjM08NW7MAqnUDdTsAslzD%2FB8OyU%2B1u0DY490KvTaPHeT5rxXdiOCc907PD6Ujr%2FoKHZPhfWJTYH2%2Fbe7xkTRg0Rg4mBobxXR7nJB4gtX5%2F0wWp2wXqh0mjd8et%2BUt7%2FFmsrL17RtDcJOXzYqtD62bRrrSEkR1FWnpdEy8rop%2B2EyQx9QVyXA%2FDSlO70vb&X-Amz-Signature=0a7166ef5c9a6c9a3422c42819ef1cdf135032577305d78bb2edec2240e26061&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

