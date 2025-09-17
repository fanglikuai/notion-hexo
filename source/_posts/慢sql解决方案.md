---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDTEZU7V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIEFuJoTX%2FypmiXP1pwaqZTQ1HyYa40jHuz84GN4nnsY9AiEAqn87kikh6Mfm%2B4c7lxHQHe9UN2%2BDZ%2BP6t3tho7NwoDUqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCaI%2BvJY%2FJxQA2fcZSrcA9iguulaXp6eoAr2MLAkGT7ykCveosTUpmd8qsUGVOXoNX9BdGM%2FfJ%2BN5yF%2BYLFNx1gBFTbuHMkmnTUUXCuHh%2Fe5LJGCVUe86GXXSa0YrVhQQkkerWXjmZ4pTPD0XlAIcoTKjqM2YXB2Z4NXfGX7UTMBjEurF%2FRr3oMjrHxSMxYOZLWqyHmy2m73T8n7U%2Fs%2BerAO6oT4cs5j0oXIt5Se%2B41StLOQ4wqdj5EsZDrohGL4hsohVXu%2FiaOD0brL2%2FcLNpz4ksrOB5YLwCS5nJCJ3A4uxg9w5jBR6zHFLiEtuOOK4yZOF8vwyYfikD4JSdJAu5ne8L%2FdIt20KYPMkChTV6V3jirntADCTz5bauupxy9X5h9hzQulNADK7KFKjP3LX4xsUy8OJgP8dgw%2FMTI1%2FhHxQw%2BgqKOwciQOghbA%2FKyE8Z7S%2BmeBFGnZf8g69%2F5SzWVR%2Fug577pUPUxkwvAI%2Fd7LUgSVgu9LEKE2JLJqrqygt%2Bx1GLFmVwHIYuoeGeT4b0bVNh%2BXJzfu0FflRuhQYrXrY%2B6shCk8aDAAKbjJp4FLGy1rpJkFAXitVeRtqWsqXM2LADa1rq3VLm%2F4kyMn%2FIn4X5lOSmhy2Qsh1HfNDSrn2tFDui0RYQFCEtBhMPrUq8YGOqUBwDCA3q4yhj3CxIkXm%2BbgwROpBCliiKOMRKzB8bnCwJpVGuAMCXndn%2Fq3DyktA91zY2HBCXw5SlDcKGb88CXaKF1CpSCjKG9ATj9dR1CzQ%2F2chAmDtBUfAZtceHcFE%2BEMPGMX0VB0SWxNOfhgLXgGg8yB67B1PpQcSncnnlwznLPeO6TgqcmX3%2Bk42pYgesQW%2FgqVFiB14LqU0HubpyJXLo4UYNPP&X-Amz-Signature=5d8591148668d30cfd8823382f22d54a8e5e69924cf6a165c08f634a7cdf79b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

