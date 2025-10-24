---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGWJRBT3%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2TAzqfVSdEBC8j7u1gVbCTRZAeXxAHyfcEedePhqe8AIhANmKe9YPTCCMpQ0ipotcr1Ejkc0xxLMxw2qPgcDhgQfWKv8DCGIQABoMNjM3NDIzMTgzODA1IgwDIHNv1wvOhyXRXLgq3APm%2B73jZhbybwI8toH2XahdeE4oV%2Bg%2Fd3GXtqXCNwrTv2g%2FH3heu0FsRM914d5GrdvJfUPWJtJ9Pvib1Tjy%2BjAoxbg2gMjB2xyA11iGVWdyv2ZUrgm%2FYesZfyiw2dsOFmMmuPjC5HseIGtBt9v%2FKXgjI362ETRqRa6rCz8cBckSbHUPJfHOcAZxjDY85FMzklcsztaBMjLrgXFweFwJ1%2FuEe%2FH60aa8gawfWeaCuFDwnEWNej8qBS8LOKkbsttAdUMnviLEBHCRtGNMkccfJkeIxBf9UchQK3m6FNSxIUKp9PRgGUbc1WvdO0eCoxSSVVsyUvx7A%2FFtiontsvv5gYD22qLnQVAafY9viLVWb3zlymIuGECbTf5JT%2BQN0g2gma9PdI2CLIDaZuiEms%2B2xq%2Fem7if9ixmZqofkJdBk2yYcyi73ihTLCUSdKSKo9a5cQvzj%2BzgW9%2BWw%2ByxW%2BX8Ye8SAwRDHuuEh3b80cU2EqSpUOTVHN%2BGk%2BpccdI84Y4cF9lRQ85hpbusgnK%2Bm6beVH59NmKe6yydZHm4qluDBNukJQNw2yJrgYFJzC8TSJUYgiZJOcz%2FCawrb4rW1CrMBQqwWk9eoMKxCPj%2BSzrW7mgd8dpDrJ3jJu8yHuslVzDQ7O7HBjqkASb6I09Qcd2mJ%2FqZTx9UXP%2B6yruY3ZDCe3mWa83EQ9Rp2F3y6cKpAOPkxhzECw8Xs06Jed2pv7wTtgRqGUjrwuN%2FWwKL7n8o80IHjxM8FHa1L0JSWTmWogsb%2F27nQVXRYhAaMvfamrIGJwDqUduRdvUHkBgnf%2BWEU5Fg8NdjwX0gdRbWGFjUmnw2TFwuAIsisFQA6irLhRqv8JqI325TRE%2BDIlsD&X-Amz-Signature=2967f775494697ac5e68fce6748dabb3b5806ee7dac0742493ace5c5a08ed1c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

