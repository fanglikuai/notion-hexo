---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCRILRAY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3KSTQ6pOCVFvSdOu2dFIbX9lixFwGbJTgQmTqUY2g0wIhAKxWVc41TC%2FTdcgutkHWzaoIxL1IeBsmV7EgI%2FPqm1zvKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2MVrzCCl2Jo05vjIq3AOARuyLm9NBksgAvBjWUffoPdwVk8%2FyVsbk8%2BOsEE3yA5QTGGNY%2ByB7wjuDxUKr9soRlxsFESgeH4LBbf%2BNbyTh0xR%2FUjsao5zy8MRZi%2FG3C5rb29VSfJzJTjZ9IujM8ch%2B5%2FoaZKS%2FIEKjvmpfdVjfx%2FcLsF7cXETwqXay9yJzjL3rRA9CwOsihJxcf4oVPgNHGK1AX%2B7ISHL64QbAz4LuXPeWxhAx0KYiCfOOcLYgt7l6qqJKOcJLsDZkyYssj32LjMQaF23y47W6jtX4PcJQUavDk8yhAYT%2B5r5beKjhUcGDG3KU%2BwGZXYX62Qji2Y4osOd7znjN3ctaEK8yOgIpCFIG8lfJeRgbjMYk0uMdpQvkl72WSxr7BASMkx2oahgAx4hI%2BtvxFgp9QRPvxmqLGeu%2BhMtvfAg6TOgbCuCRJOahjmY2zyuP8VnOlW6Hh57TGGpLaDEF9rPtPDbw8I%2FyASGXkouBKurkGILT7bLH%2FeYv1HdVhFZPs9s%2BoFPXHgCatXQB79WDhuCGLNfGDGTIzJIFGeLcvxCu3gznpjnTHIWvUs22J4IvQoJZQQ7TO7DlX7P8WqpGwQLGv7Y5yAq18SHeCBBrmCIZBnosFKDdPOk2rci1E5xaptp1szD8wqLJBjqkARcWeUT4zWmXim795npL%2FVOcZsr%2BW0WDivtQyqA4x6iWiOZ3Rlgfan4r2cmTNyLAWesUuufpWkc1I3VyHkHV1iPI1InaKHHjH98UEohXxwV1htA4IyyKtbB%2FBSSsjjWJth6iBucprqBS8u3uP7M4cddAeoGLJfY7DucU1Ih3PvMndxWZYJc2GDQa2daBlWiMWxQJE0QsS%2Bo%2FwsznbiXhskEhcyPj&X-Amz-Signature=c2e8105c0db6720b4f94d33a4f6a0414074a11db274f0a0f94fc245fcdffb889&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

