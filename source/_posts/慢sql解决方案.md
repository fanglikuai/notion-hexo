---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGDKKWNT%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjtU5IKJv4lPPrVodWkrQvfOC%2FthF%2BPPOvqwuTZVBhOgIgWycs3Y7Ny5hRq7NEYWhDA7j%2B1dDvFgln2qSB5iekywoq%2FwMIKhAAGgw2Mzc0MjMxODM4MDUiDMU31cHgWaVsnsnsUCrcAxEkcL2xT0z5%2BUxjrUDK%2Fkbd8udse0hoLlU2yJRp5aYhNbsF%2FS0soqEZHf1yfj05ogOkcx2uBjDyChzMBZBsVfMNMLHNsZmNm93GOccyjjTYAxENFEHRjyOH4zBw2vaWiKzpiaNqqLtE3RtK39fb1s9hcMz742v0rzDTt8B5hPqxnc82fUkNkvCkFHfKrQjpwNtL7FQzZBjgFoM1DfROvyuOGC3PMam%2BmwGQ%2BZjopgLDZFq%2BKmwju1KLT58Pd1uJoru%2BS4C3dcSahcgfUjpuq84dI415HD7eeYYl5C4wh%2Fwc1NiG01wwcn1nnZ2UTifyP%2Fs86tIVJVQahOP1fGUSlIRkIBL1Cae3zv5FayzQgNwAEu5Hpxe%2FDa5VyzlGYYYxRpMAFVmC9y3%2BAk7KEcq1D4hpFJUpZ8tZjmLAB4ZuucUG8EH5VqYVW4sWgdaftB5J6nFqGzRaqt%2F6tOCnWiKJfFHceQXVAxiU%2Bdr7KnTTYmQ1M09IK9c3tMDz1%2B0rAr872K6FJDi%2FfstmRANyg2%2F%2B4xg6Gik8aex8y4aO%2BDX6vPsKrz88w7aGYIPkJ6SAW206oBMSZXJtqVTe8UVPc98ff7Zw%2Fm4EZLKQ4mBBdLS2oSUoPEszteNbmGthl75sMMSdxMYGOqUBYrGk3elpqGiVtFmtOycsRE9fHwgUL9h24YIf4UL%2FGPoJEqPjexpRqeva2FTiV%2B1%2FCBE%2FD3lK9yU0e78WSWxZWqAqrZr65i5mRfSZhKejxAWr61CUXxu1I%2BoQOUwWWK4EQu5gujd8iRGEBFLb4LjXlxL6IPNGAAcssfIK07M0Zt43Z1BzKePu9EdX25cyzlBpM02C8saIPwjFdulP%2FtQBPVh0pn%2BK&X-Amz-Signature=8677144ee732ae39cf9a93a941dd3f30969cb28a1762886d2acece5a863f93be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

