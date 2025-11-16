---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHCSDPPW%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICIRp5wG9cYlJir1nG6phZDE7H1FdyxD9n8Xuxfg91trAiEA0Yp6bIUKou4wkV7Y88H3keR9g1N1nwadGb2RCkJSmysqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPjw2sQHGlG%2FX%2B1M4CrcAz8HM%2F0HSDRi9ygN7jRaHw%2FCDbiIjZP3ATNvtKupqoaS2rYxAK5%2FS5e%2FGq9T021ks9aYrtd2ZFc3XyvhyY%2FpUkqkztFUD79TYUhJ1TFxHVS9ERAuOKn9DhOr6%2BVO3z0rhPvlZnvAVEezzjCxnldxIgTi0yWTAP2b%2FhIaPnbwbAQE1CMG6Y%2FRYDa0%2B3ZsFY0y1s%2FQAz5P23GrtqiHNPfE5S%2BCEQlhH%2BEhTAAec5xovIFpyO0%2FpDYzhopki4KOM28qSUWHpDChwDG7jEK%2FsIjmfyWRGS7s%2BbNXC%2Fhhyfe0rC6n22ReMZJtDrc6rmgFk%2FXieew0WETJtM1y556g1PRlqdVZb0jKM3sOJfeyEq2PLN24Rei6iW5nZdrC4dVq7dOtsDFX78wWDhmOf%2BN64cLRiGXIf6zKS7KUo%2F9Nsc6Lhde2zPB6dsuI62gUy3DITiF7y5eytzwO4forBVVoasS6KPDqkF0TtyulDhxQH6kUJMthEwnWNAVgAnRcwqW0EQ%2BQCR3ATiKaeZRDs%2Be79aWxOATVOtchNJRu3vblTGSuXk%2Bq7ndfB75YFHwQlCXQHX4IccaQwcVRWUXMNHdPE99cyJEpsgkc44XJSxSdkP%2FGiAQD5tew6KEcbGL%2Fen6RMNXe58gGOqUBA%2FFpLeX6Cq9cxCZ2PxvyJFDpXVSCaGxCAPskS5G1qLTqBrj3Uco87pabQkhaGgLV5jV8TpQYcLNTes3s%2Bsi5p129n%2Fc02hpMnmQHhTKInUg9Iv9G%2Fg4L%2BBOzyb3hzfdr4zOY8eIAlHVWDCZkwyV0pmziDjVgoWBUHUGsnwDzKwqVSt%2BPm8IF00zejb%2FCb8Ul6zdEWhY8J2HwSFPodSHoIreZ7Ydf&X-Amz-Signature=458d4f0dc08e95674627584f333bb567cb8d54b58b245acf9216811475eb8cc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

