---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2R4LNKY%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T170053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIAivNQUosGAaMF4jpd7ZzC21gPho3sUqObZq%2FCbY2ft9AiEA%2BDyHW%2FkXEO68CUWkoI%2B06ruzDqiGZzJ1CbwjbSe4PdcqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvJ%2FQ6s5C%2FMm0JMzircA2VyGzzY4i7sfmtXGfqxE1%2BO7A9BCfa3VJNd8fN9iD4XzzNXjN2VfjSmB42%2Br9IbwJ5weNMREUotmM%2FA4mXH4YoY0MOZjSULpid6ZVgI%2FogrJjNADqm%2Fzk5rV3%2BkKvqR7o%2BOvhEPFt2pEnGwuvPdTnwbhgOLXm7esJJya%2FyC%2FstjkVTE9184191hQYe01zuajtjy5CrFtrAHy%2B92q%2BNZIOJClUU%2FEhSVuT8Buk6DKfNg8TKeNmflR5kdrsAjlF7nbs7%2Fa9gbKoFSO%2B9R%2BD0g0MfbTeBH3QkopyLjsWh0oxLJrTjGScAbWsngiu2%2BeVy9iRpZojg1BJnjxgHEeK9EWUqJa7iwgUZEKEJG%2FrR8jgD26t7Cywat0A%2BJqhfdes4xWEG1UrLSqv%2BA6m4Eq3t1lRywzj6G6P3cmSEK85DuRiuhMz72inFcFNe%2FTnFHTjUwoqUHmJWibFVHEj8jxZ%2FgYLOte15d2lSApvjQCN2Ds1ZGPYF9TPVndwiuqozDTF00ThxXBBI7j8rk2lf8GVT8kE%2B8mx2Lr9sarMnkwsfHYPV8FoYKGjxqPlfUTrKy9nH7FMwrOxBDPTBaH%2F7myTj%2BJLL7WII7VSIQu%2Bso7didUV9stcMp7zIsb3Fjxj9yMLj2iMgGOqUB2a7uwjmnyn%2Fg%2FZIl0%2F4TpXp5vmwVTXNAiLjVxrJCiMlCn7OwdID8NITx7cBAkBCM3NnhqoPi%2BjC%2BPPvgE%2BBPBMxvXH%2Fk5x%2FRvNoInoOUVa%2Ftb%2BiW%2F6tl4zIf4dsfXbiq%2Ftz7wMC%2F6kvhQ3yi75ATQ9hYq4WRpd7gZgroSEvoWOQ2%2FRCECms3O0EyDUJK4m2EhMbeljoqsstf%2B1dQg959SId8TWUe&X-Amz-Signature=e4e0d7a7f668c8f7ce9c0b9406ca75889a556caeb0fa6e738a6ab805ad519ccf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

