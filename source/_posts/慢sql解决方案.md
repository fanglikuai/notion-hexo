---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMQMC4FN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGwaMa1yJ0wa3ay9faNJYsIBtmXTb3xm5dq54tkgKtyHAiEAn85FF1JNY9h%2BSqCN%2Fa3vfbbGKsfRUdvpgAXaY%2B%2B5SZQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDOiJmE4kwCSI37UyoSrcA5vYPnjThvsFDcciRNqKB7DMkPRo6d231cDXm71I5ut2QKAO%2Bp7NYk%2BsT1nRV0rjeseqj6k%2F%2FJHg%2BjdoSxRsrdJh3I%2B9gGMsourXafVF9WhWUZ8Ln9HzZT1ypeihaerI%2FT%2FIpY77a8g%2B64AghPYTuqnS8XBs8bwbnbOp8yLqtd0g1ECm%2B4Kpc6AsCJdQdJ1A6Hn0gHP9344OWxk5QVX%2FwCtKmLFCuNG9%2FRQV41JjOZ3t3cDsiHlSvk7dYTr6NOjDW64%2BLnMOoymXipLZ8GG1PHTY%2FXhE1SQsRlB5gAmgjPwj7YldIEw3D0iYvQLhKw%2FuP1ib0QzCu3EKlajds5h0otssoK%2FUTi2GbFjKhSuwaSEDF7O1%2FQZgHrZHTbdUqxTiaeeHot7xzlQttsk0SxtbPrssujhsEqxjtkuQTlGdi%2FcpoBYlsF0TvgVE4M7B9XCwQ54q78BohbOMpWdBLfAm89TkZ6nqJWEVYhleTHCWR6BVuP5PGzlsFcGLnbXfRiAVoQWuSMV2j72ZMWyPGQk4DvgE%2F9Vv5Et4NvowMyvmFHgzq%2FPgWl54NQhvaxNtt4zzJo%2Fto8SWdb0F3ZW%2FsH5aCysRyjwxnlLD5mVQPeXs%2B5QHEvqmJD%2BgWI3Me%2BZuMMDo38gGOqUBJZviKTaPjIwex6J2O4iIvXzVt3troGX9ihevoMsk2OuhugF2gzhOX2IKWo5aQQ7kc4wkIjP9o00eLVdV%2FTgWaXBgjQ8N61OR9Di62%2BTABVlrwzzcwzodzNMvZzE2lcKZztRZF4tc%2Fv5Z%2FvdPXc1fWTiWInrl6tYFbdmUxY1DxW64A538qvq%2FzgXMZ%2FpfvB43O6jWdJBg3sW8iK4yTyAtX33P5PbB&X-Amz-Signature=ea7154fbf1ec90f1b200550b213b4549c0341b21472f027ab939ec1f5fc09043&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

