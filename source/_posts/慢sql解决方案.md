---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666RY6W4D%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJIMEYCIQDDJLx3%2B%2FdiVYDC8yYA4dl2reXiCX0pZSb5vgkm0DcaCgIhAMSENRoTI1t6oAt9lJWL%2BNgLv96eMIUWx%2FdiDBY95cKEKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3jAOKA5g37tOwSD4q3AOLrWd9uqrYE%2FDKZvWg4NMXrKZNtobHraGeLe5YuxKKBN%2FHsZRdRO2pWMOjoRdqI3Nj00I41Q94HJ2QwyUwdbL9%2BOlw%2F9LI0sMJ8o%2FAOB58y%2F%2FZ37yAm%2BDfVHUu%2FkxFeXxxiOoR%2BrNDGMORXf5ICEekV6SvOScrbWBdaUiVcOxIB3yoqd9ZEglW60Z%2FG1thA6Cl5gBywhmjCXYfs7cY%2F0xcqQyI86B%2Ffd9y4EozyVIAJ962exhQpXy3plKFoRgy5juJW%2FvdtjyenzOOeUG28h7UefpLkAPG5LjXEa1JlUTr%2B4pYI7VjVXU5NdZYlWKGkgiZg0X6IrIWScH5qPLKbfErPJW9V%2BnPO8QVIrISt8Dvw%2F4nRkZhLTwIFrKFTtN012PGhQI6EPB27dDgwvNHMsiumbC1vfina%2FDel4lVoMqjhd%2BPyOiIKsHvXhkqmVmluIzvIwpIy9XRhdmyXd9mM0hDz7i1OMzt2469NqV7dBQfqHq%2B23BRoFDG1TAqBJoG2q5deb1Gpud3QQQDmhvyN4e41GQ%2BiJU3lmNPXkL68hOqsJCwZTEMj%2FF%2BgemlYhMeOndITw6vjJimDRQOkwdmdtVFQrOWWFWPxr5Md2r80pRyteidoAV%2BhSegKbj%2BpDC%2FmfrIBjqkAflG7iNGnTF3iF004V2XEAzZEjoiSI5g0qcahMX7Y4QlsbUp%2BKmozBBJp92xPLrJp5cwleWAKAuCoSNPPEfsYBfA3DI%2BPVBX5smHCtocftr4P6ZLDKxHG586WoLN9ZVyQTAFvl2r1pFjdYyl6DA9EulOwEuX%2F4v9kh9jhhHhBntYMGSdB7EpbT7Oxyjkt8A0TEaJWwaQstjoavxI04Hi1oRUaUuI&X-Amz-Signature=f14d0ba0403bc8f440b59e5d0c53619deaa705e20b4991003c4240b8f85d9805&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

