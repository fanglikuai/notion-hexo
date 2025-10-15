---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWG7ABJN%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEr%2Bx%2FAbCKzez4zrWs%2BlXQKuXmDFjKpjKg2keg6vgww%2BAiBenvqF21BU4TRDMifgv9lotxYta%2BWEIfjrgH7f%2FiVphir%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIMOzlqRAm%2B1T7XvooxKtwDGauTBNGs93JQqvpurUdmG1%2FjZ%2B%2BhO7MeLzVdI1NVUEhoWHhD6dZ5HhuN7IrAZNpYhKYi45e%2B2iHaTgQNVA848rmvbCcD2uIr4U7jBiubPwPClN79EfonBKNiwWWGGiL53VeoxVSChw1OrK%2BRSdZ0qB9oGcWTSxRvNozMS5REibQtkQxtW2UVGhgV%2F8M54gl0nl13mTmpToNcJIXv3wXPq9AdvaUnr3avl5z2INhC5PLRVLQ5ubyN2R9PzwZPGRUTtLpb%2FnOxny3sakIKqBP%2FPkjbP0ZS6hlkjJaswWTK5PmjrZySJIRCFFPS0CAv1ZOuTPjbyHloWNVYk42aRTAQvmUH04re5T%2FzBM9FpWHlVPAVbz9KivCnIz949OsFf9hiZN%2FKtadhuEpPpRtUpRzwA7FbrAOuGiGzJ49d%2Bhkbqorf8INiy5a4VKqTrOvs9oPT%2B%2Bg7zRxfsnOSxpIN0wHPKomWeqGy2nW%2FynCxJsZs9M4OE5XrGeprPwzOfDbLuKOgLG8Up%2F59wjmHZVngZbu8ZxE%2BawmF9ySzVW4yeKyl5oA%2Bl4XhlEGtgeMwU2tOkBw52pL5eNfuGI%2F7WGtmJIIVHU1KzgPpRp3llZXr42Xrj4mcjnkoWpx2qjLLOKsw2ua7xwY6pgEY8WRbCUTpony3utw5f88CTsCXCGyhPyMEVlNM0iCxL%2FKk2HXg%2BSW8vg%2BKjyNOlhf68Rvoo2HWRKYaaUW6V0DF3pKlIlmJbc03fV1MCmTUoHkmxahjnmTnFD8AQEGqhgCVuyIIFr%2FeuxFju4Ylsp1kdJrozRGspGELnT03pnUEHWBbFBdlT1PN713hK%2FPU0OzKND8t7%2FiGACx2X%2FdT4w%2BKhzUmxlQl&X-Amz-Signature=c43531b8087dd74153813e6f40dd0a24fcfee9742f24e16e3baa60223d7bef7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

