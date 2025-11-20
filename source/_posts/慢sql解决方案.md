---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJBM75ZF%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T200047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJHMEUCIQCoE1fNhWBtnWC0JHmQzLoM8S92itYuQzuN6Ofp5ilC4AIgC3s%2FuzvZwZGaVeJa%2FDKAs%2FQsLTuNOPsriL7ARvQGGgIqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBKCwINty%2Fsgw2xmGCrcAznTi2STrikFVGxLReelVpLUyJLH6NxhBjTJ%2BHEnmJwJnWBSorpkc4m5EBS%2FxL0OEg7T%2B9xC%2FLpLOgGs1Sfxf%2BOdC7c%2Fx5VbgtEqt9w%2FTjvKY6SYIIm5mpoARKeRn%2BATB2lIZSrFQ6uZUHPggMu665zcDQ%2FCrENkNPhPIfGGcDn39NO1prTfhXjWN1np11r%2FwiPrIqQrllBjLC%2F3%2FWMspZ3H%2Fr%2BLndrs5jWaJ2Fjy4ljC4fi%2FsR4z12wC1jvt7DouTOl25jymuhDnAV3sFNdicX6UGwOvKeY9TwgC5n45I8oh7WayLjHI%2FYwSUYOq7rXGS%2BpZGhtVC4aRjl3h%2FuF0eh1UoHV4sK7kxuvdvZ29C7XMYosCi4G3RGZaSEIIWLLBLijS%2FNB9QoxieuQffdM2hU6tCuEj2okaxOUQahcyTpqilp%2BwuwwdGGfEINRVfqgFJ0TYJGAa8iwuFfwQw8zEUM65z38pp0yh0%2FEa1SYh%2B2uZhCKRPsv%2Bz3alFACCn2%2FiDre%2Fp8E1deYFeZ72WAWxwN0FZvGKZMYmTcdIXiqC3kYrdoifmIEcc%2F3VPR9mbV71Ks7mS8qePCdaTHYrkj06jNH2pLrdjRWi0B5xcH7778yom5FfYi3PZc1g1tGMLbh%2FcgGOqUB5lPvMAO9mOE1VtWDG%2BGXTN0zFfnu1S3%2F91liaO9CZWZbPpHqYOKXEVXX1wNOrYQs7rz39OKg8b8RQfHSfrZwzbp5YN9uo4ec81aPhClMIHTRsxXXeK63V%2BxXEla7M6A8QqQUzFNlLFT3NiPy9pTENmNeY%2Bw27%2FTAivmjRCDQN78iZOw0NBUdhiJpHnxtKFlbLynwELYsTgwvmiq121NVAolexkg5&X-Amz-Signature=3e4013b07c189009d40fa0b24b5522c3016f17343d2d09eadca1268b3435bb3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

