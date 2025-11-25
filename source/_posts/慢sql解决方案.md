---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UOKHPHCB%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAVOJYToJ80AwPuNzgqMp0cAicRvSSUC%2Fb1WXAr3RdU%2FAiAr7GjXbe0RR%2B2qZrbcyc3CuELPYNUF9PEi2tlCAjaBrCr%2FAwhwEAAaDDYzNzQyMzE4MzgwNSIMUG%2FPep%2Bf3aJFw2NAKtwD9rRCcOCmRbxbPXGIhu8Lry8f1QG%2B39LU68YdFCL5NVcXoxCG7HTOCKqj%2Fb6PqQ4MLdmXQuKSoJ8SjECs7mU%2FOHadZA0BeRJkx4stOeW87rO1HsQMGSW1tnXTmYY2lIzBRGBG%2B1zX8jvD1UDVOFFvM0K3ig30jFyjvoBkC8PWZnSACM8mCLjvHsZd7ZqUHjR0kBiQfpELiMfxUNFcHcKa1wochIlZehdzNPPR7%2FQWP1T7Xa4ZVD1wwVv2ue%2Bt0ktxcAeoTJ%2BhrwnQ6qTE39ZV20kzDLB7f16JhYF2LDsjpLKbffut4PuH%2Fk2bLl9pf5KMcgXnVfR%2FPIIli2jCqABY9qYcBTy%2Fky54M7AC6wDxI%2BUYT0KpIA42kQnOyWPN6XosNfPeFrPXasaRTGHQzwywTRF8JoFHzecZ4oUxctgMtcAfhcUR9LuEI2bUpIwhNqGdR7uScriVqtXzKgATNMUXZpoYazAZIdq%2B72Ch8AaQMPKJs3yqzcSZ5HbBnwobk98MSKX1hOBXRyAQuxArQTeJX8nfMu%2FnFOYRlAwbMG4IIJNomAJP0p5xwRWDGfywZs7KZnxK%2BfVDiCckyGFkaPFwSOzfvnb%2BhLPofNIRowHpBxzGqwJnzmqggMwPvLEwqYGXyQY6pgEiBigg2oMXik9KkEKaG3WLrzmyJz1q3JscqEO5WoVRnTu9PBeuWKv37Z%2FKJzW7iELcyetOtdhCOvV56MhFDA5gueksn9MikV%2BfjcOLrv7VM3r7BD%2BWCXWZPPytRS2D96k3V6QCuTvCR%2F6NwuOoMTnFRt9jv0jhP5Rm2peuhsbEs%2Ba2ASIxLqA3Dm61QawfhRaClYDq9Qt3ERbms%2BEU7VC12KKUz%2Fy8&X-Amz-Signature=0c7fc0bf449b67afacfa768c52dbfaee037a2361751f8b9cc78e914f0f68ce6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

