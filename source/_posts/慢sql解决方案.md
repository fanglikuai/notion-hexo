---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFBN7DW7%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCeqVSCveuOWltv%2BY5IHAMHtp5aI%2BlxN%2FGmMotksDvMowIhAM%2B09GcyRNwrxcmTJ0Jk%2F%2FUJH5n6FBmuGxqICm2GSpHuKv8DCBMQABoMNjM3NDIzMTgzODA1Igz8Gzt5MbJifcLLCeoq3AOxApX7AvOamiZi%2Fuk1yAND4AiyUNiZw5I%2B%2FNUOSDBlyhpWs1ibomfUNeonos%2Bz4SZz%2B7McX8AhLcqTBINWxWKlGyBY7opEPnR%2BwEK3m01l%2BAyEoImydlno77T1WG%2BAPZModJ%2BKyc4dHJqaHUbswAZutex6FWXYIYOGeRDfKUZv%2BV2gaTjoZ083xemVmWKEfsuDYhwBcmA%2BAjcrsvNFrS7trBlVWP%2BsuTBx0TZNPtNnNhxcCZYKXjCVt3ZNi4nyQ%2BSLocqu7%2FZMe%2Fp1fBRGpTCZktM9YshyUiE4S5U4%2ByRyj%2FU4yTo12MQPDlFgMYdLoEWRT2W3ZEReFJR3EdveLvPy3gdFc8A%2FMo%2Bg%2FAiiaG0HE7PIZ4wOoWDwYT2vvuHhab0IWaewNUJoClMFPkBIg0PX3WysIz98P9DgM%2B5aR0LutirjZKqJ4v1T94ziAd1vcPyPHvLSxGC6oFC2S79QtSG9Nlw5THHiS9p3uV7kseqLyhyWzf5smooAKpdOJqW0MENi%2FzLna5wXss9%2B0N8KLhrw8eD3JJY%2BHDCYZLQWA88kP2YtKQfZR3XVPhrDvAGgeZJqZOasVvqR2wJlzKWZXnM3KTRuqJcRh6KRHg%2FYLSBD7ZRVaOr%2FdfFWPtVtWTCc7vPGBjqkAZeMA%2BweX99WfC4HlcV4KE0zQiXtSDfDrhvnP9aha%2ByFSul%2B%2F6sts0TRnqpPa1%2BtK6sz71bgdwMxYRpk1lOimXOJ1YVkV4SbFWMj%2FdDSafrgz0B5GIy2gcd3hTVjEcNlloe6lSZXscwBdm0jrJnqb1RzLO3%2BjcDCVj%2B%2FgAvKlbULr9wv6DUf1PdmEYPShrClyM1uroQ5%2Fa0Jmg1gcjrYGha%2F5ugO&X-Amz-Signature=b185e6f0aea285f996bd7de91d40690fd13eb809b073d64b3978eb60e64a7e4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

