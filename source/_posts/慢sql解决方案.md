---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZPEOUIV%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCI0BSyGKOSo5tKQVGaE6yMSRtLDc2u4M6uoscsaFJfxwIgBKbISRGICGbY%2FctFDAoE2mbikbIM5xzahR%2BOvXjGGVsq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDEjJQlduw%2Fiz1BuyoSrcA3MSzaNUNCWh5Qsert%2BZI%2FlTVQm8Nc6NvBnGVHZnwJZj30vYiX2yciwkW%2FNBAnCk9M%2BOj2V1wTxb%2BMfp3rFWjEKnoESMAEkX5OFi9%2BbefEdQMOArt1kEAU0uuOvwcDXk81L0hZlL%2BOnsr2LNHKTUPFm8kM1NMyYbKMsNIcui2WGEGa0psTUnd0DhD%2Fk2kmMbedleo%2BT2AXTzgwMcNkm64W5FDdrpQ9RPULYTJw5ZrLiXRhD5OwwegSj9MhcK0no%2FERk0cTSwehNlyGBNAbPqfNKxMXgY2s%2FaR6ibAq01kJKBoB26XyYLD1xYfhUrEowK6yN7wfPbj7mVuO9ugYVB1ebW4C2GG8EW37bqv8Ss%2BsqK1div41SA7D2a2Cpo66N4FnH6sdmxpVOKlIRoH%2B0BQbA1H%2BdcHE2HbPrnVib3Sz%2BcJ5DKOGr%2FjvsocpccV2EB3q46hPaxCNICaNJD1Dw9qt4Tz2QEFy13QM9%2BMy8ENrwtDqkvtxCpjQ04RkS5frk%2Fzzi6B3waDF7cvLdNdZaQfDqRxjtDlzHipJFx%2B9kuhVlQkZ3tRZDCE4WX%2F9QwJA3zrHob%2F8%2Bcd2q1pBZrkN7zlBysqoTbogCl7B6But%2Fb30TUI1s%2BHHZ%2FdBU0Ebm6MJaalMkGOqUBaVdl%2BrLW7DV1gJM0JohCUyOMF5qCsW0j3t%2FJ1ENr0z0znIZwI97IlzeDCtAni8%2Fi%2BxX4icxKPgzrcPgAhP5JczBNJwXEQOUPznDOW7v6yaSNClqUg8ZRqUCbFOz%2BLsFBKCTUIWlq5VHgVnFA4XddZqnw7Q8DB02fNy%2FvNPcqP%2F8wsj6q9emelhBsTM3YwT5KyJTizsaIaOn5DOE%2F%2BCHnD9Zh8mKF&X-Amz-Signature=92e2d1108aa733153db2e9e040f153909eddbd0a46bb48c61abfefb2096ee0d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

