---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYDQEPZJ%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfrDUH6B2vQZ6hay7feNn5b2x%2Byt3TwSdJ%2BQdiQ4OnmAiAmeZNelP2HaoNXMFCHKyuT3mC%2Beoqt3EzGUxjaafMCqCr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMJP2yLvyd9R3wSwKxKtwDT85%2Bv6ESD9tg%2BM5fFI6QFOvJodY8%2B2Fhn1SLCC%2Ba4Kfak8MwNiT8kHFwIqxyvoWdJUZOEayhP6E2j49bf2lywzi6XATwFbH6Us9sbuYuCdJ3inuTfcWmO1mW7WqG6vqbOUwfINmJ8dde4%2BG3IBkVDq%2ByH6Ul5g%2Bag420pM5GeYYFVTVFpY9RBS%2FeaI4FgDJDGL1hDHXLwozvK3hOJLSyjH0%2FIlRctn3R3cdmUb9RxWgQKi71NoCvBs8fk4L2hqXgFXoGKsmcIAle4DHmNnNYco5lHi9Gv7moiRgyw%2FIZ2nhXdOX6DasOoW2b4hNyP9opTFh5QT%2FGxQTMc43Lqle7IdVZpem4UfECQDk575rJPPQEVK9gP2sljGi%2F2y20JVEADPPl2PfSRpjjbxuFBYeQ1nS2cMH7LeaTnkyQiAvmEUDL%2B8MdghQbzaHJkXyarW0aA02jJ81HOpl1nOw3igg8DhNvFbPwu%2F4Yt5PaLrFYDa3NJPSfavOYCLh1DJrlmQgv7FXaAt%2F9e0nnHZOVunFqbJ%2BaOnDx3hFYdEz8UrWUWRDP3Au0J40CZYKbQ8KqlVpTF%2FQkg%2Bvh0dhOOEMRQsq%2Bp%2BPwm6F9NzyKPAQrBGZGVA1pcgZP%2FAMmdv47CDswj9POxgY6pgHdelQ%2BJh8VVFo%2F46BXMhEooL5pXvoNJWMt59hzNsVAJUOsozYMdrBcWrIFczrZwtUVrPX8k5iPYR6%2Fjl2VovmHZ%2F9AmF0M%2FmgJoaBfbEvBvdQuq45cRLFIxEfC%2FYZbfrFh18GnvbQDOcD2qaoq7SD074poajvfsfTkUmp0%2BNhdKr6hXt81jS1nbqCAw8HmJPJkgLY2fn2HEoVDpx7gQJqK%2BYXBrxsd&X-Amz-Signature=1eb28522748fb89270ae39d3a3f7ff400719e4e056c45aa54771e1fc41052e54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

