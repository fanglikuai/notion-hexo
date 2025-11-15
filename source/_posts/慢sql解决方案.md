---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY2ECVE2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T170058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDAsENdkW1uJIuGoqLdw8w%2Bypt7CSCroYFqhCmpDe5taAIhAJ%2FE5GrCRGvMrCh2ZVADa0zSpldjCo8mfSNb6wcdo6N1KogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2BZi2FOtzfae8Bhl0q3AMcHGS%2BsXyKmxEvns3Lk6f%2BEJDAPbWZ1QRgMJ%2BOTd1fkejGF4K6O7c4syOV%2B8QJCw9cjKis6d7b5PzcJUrkK3mDVr05xYPZmYKehUKPqetzrw7cITAl9MIpyPBJshRK5qdWf6r71RWZH1LSO41K1rHWJtZcu5yCQkAHdjeb%2B%2B3xZfefGeQYoNl%2Bb2aHM59bBiFjdcteFsqioDQtpqdqoSLw3Z3BI8dl8HlgHuoGERgAthwMNpdqnx0pBmZnUldefzuwJly05nJ%2FOVQwzCwCV6Xlu3mPL1Gqvw2922eWB%2Fa8wUAteYE6pzeVtZOnDiSlH7DgPI98JMUmhMKn%2FLDzjZhba75aXhxjq0yFE819lQOia1Uk2NITM9%2BaO4lei%2Bx96sM1V6yLFoXBVYfPWgubbisAF0HSKBfldcInO1%2Fsggw6FLgQQ8Y56%2BgbUwdBDQANm6A1Mn%2FYeYuanPZA%2F2nPRbIlt%2FgiJSS4aedSfh5Vabt81jf%2BYn3Uqy5%2F%2BqRuL%2BIput3QUr2Or8ZvBs3VU4eV1CJ%2FTKoPS47W1dsoYmMJ5%2FXZb2Hnmr%2FYsGRh5k1nKGno75pLpTZky2xlggQX5ep40Hr4iVxtK19RkSPZLxdoiNCQ351Nrq8uFw9ggQEC4DDvn%2BLIBjqkAR47FanKyIIsCD9xXclv6Vp%2FzLECifw3VlGI9xMQyyjtx8SsLj2VrKg6tdJ3wEz2a3zF%2FNVuafoTR0f12x%2BtvU1IDJ8TbZkCsmeH18yOwHinzefgcC6um7bmTgmXKKQvtcp1N1ZUXz6CPhF5yyiBg6pt22PCDIalZHzufC9HZlxHH3%2BEidmAjz5eXoTctgwKawrq1QImAUtxkZOLxZ25SIxogSds&X-Amz-Signature=da379be85e39264e4057157f7203f2fbd463dc378077d5664be39dbdebc54c4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

