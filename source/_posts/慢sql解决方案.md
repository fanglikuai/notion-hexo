---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RXROGNR%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIBUYjZ8r%2B2Vjo%2FHMPY%2BQTYk7S8aOF3VkPiVdzrMpVdkLAiEA19F2w%2Ftm6SSn6f%2FnOXnvImGZT9GyEPB2OQptLfiZhsUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLJ%2F7AnpgA0TO3PJvircAxMkV6%2B6%2FHkqkG%2BzAUQZ5bpzpoIqBNAgQp1oKwiHubXRwFUzISrcytosi%2BwMFe96I7dNDyw0RSPUchsmApOEMG88LdBSJ2WZxyhR6NdKgPb9HhzIasCjfHfvbeNeSMLBxpWSiZL%2Fjn9Ze1RUcnkXJX3g0bAn%2BmBqXCQfHFVuwdnEeukF7QtzJXjmkAh4iuquz%2BAPzS0JRUKzazp%2FhNxMYwsU1W5vLnYHY1Zre3tVsezQzNRTXWFckC0tyNXHVXWGQoeFchNIgGrSc5IilquRtG94fa%2BZZGkQA9%2Ffg8FTY11B0g1cC0kKb8foreLVoOtb9jTTA0RsSIeI41tViiuiuRvJsywa%2Bb3RFzeOWDNporiMxrlsn7nEpvKp01kqzG6tJNQHCLbMB4wzPc4Q%2F4piJGbz1nFwym8mR%2BOyaucsXagy%2BR39dtJa6FyeNY7i0F3IVlVGFvIPrJE%2BDu2H0XS6L9T31Bv9yB%2BN%2Fx9LFLXSKjAaTn7gE6PEA9lAWB0q%2FepN4VmmGwOHQCTMZ0bu7PXsv3dKcRDLd6cRrw2Lv%2F6tMva06rBg0OYlTjecPfGuGyJXeLkTahTyD9T0X56NA4%2BFZDkW%2FU7FAWwsqMojkRos0%2F%2BcFtxpFc1JJvNnWKpkMNnZpMcGOqUBDqqHxRckJkN%2F%2FrafI7IJnzQxlSTmhgnD1Fi5neYoTwRXG0U58Up9VBupDt0vZdh549bLs%2F0biwd%2FF6VwkBe%2FqShJDkanrVuxYU1iM9e5JFoCArUy3hfFbvR1EFFkqpeCyqaSIKYKzAORS6kxjKXPS5C2jPkxsqVPRqgqiUbY7VFpHZLWDMZjKCwD6mrD6G0GXZA2Fqw9vqBI5QvbnB2MLNa6X%2F7f&X-Amz-Signature=a9fca40a4275b0f76a6047c91cf0321ee9054d544f12c8294e9239b13b08afde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

