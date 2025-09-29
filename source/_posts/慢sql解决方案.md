---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOVBHCGX%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T110128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIFDsgBZj8tJCRVP9QuUfLNP49Q3g3McDDVKBudMpIvArAiEAsuhWE1ZIcJE%2FL1lb8%2Fc5jEFMqIueFLSDuhgutaKTPCoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMxV%2BW5omuWcMo79kircAxKV7udXGFQVdg2ApEzRKwGu9gLSw0Wqdw%2FY%2BUThlCaCSlyeN43z5kW6oKeGgiNfYdd4nuWhqWPjLOOB8sRu095axl22e9mZJoApDIMmLqD9XBIj6%2BB3CjvAVP7d0%2B%2FFMfVbVKZivi35%2Fje0jqepJA1X14gb3LWzhe77AwaDX4misPZeCUst8Rpdvvmh9j2sP%2FjawnTMgJ0oCb3ellmDI8e%2BfOZcDcOLGkeUKQuFgupNOiTL%2BcqJDztMAQEC5hjBdxvmiB8psbO%2Be0mWlgB5fafcJJfGuCs%2FhARyVgFNcJ0aAdY%2BaQu2CH64%2BR%2FlmoKpkIg7ZmSWAdJxgPuzsuaziqyll9dbea1mKVsBAHn15tWgUW9Q7z8EfZu61i%2F3mxWS1Vomku7MbHryxAJWOFvWkneIRydUH%2F%2Bvu8J4udDlRa3U6H2bF0%2Bdd98Ken8tvf3Q0aXnDUdvxhGuW2mEfJVpoqE%2FMwTETVSKnuXaCmNXWyFML2pkAwCjs0n9A%2FWFH8yfGd6tgtQwS0rLMXQ7vumMV6HrhjSXrcSPNpQz%2F%2BsiJ%2BctsB1jBOnRfUquu4hRISbBXgEnWkULmX15la%2BxLg5CFCCLvqFdyeCM3WSNcugJ0KuyHL8LBojPQa%2Flhwa3MPey6cYGOqUBV%2BPlYvAHKPqfv9N5F9YjnVrECggvLZBv%2BWeolXXskpDgUglmG27o5lJgTHWpTBPOOXN38WcLNNkDU6kD06e%2F77MVhPPmYFtyTtUcoJPoVAiSzMSPJCTDjYDh7Kew7XZzVPa3Tsbjkh6SuFEYmMdR0c57bU1%2BwC8OZfY0dxJQo8zDKcM3wwRA8Ewu%2F6BSGXMP%2FvcHsYtwLUbdeuRHjFW%2FzJMDEG6%2B&X-Amz-Signature=7c6ae8f5a7d12f0f0a1b347315c2ce929ea711f6ac5f385206581d4a07b653c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

