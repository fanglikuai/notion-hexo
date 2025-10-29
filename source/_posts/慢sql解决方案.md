---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPVR7K7K%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCHHKjwtr%2BuNnZO3%2Fhz9N7BRAo8jTnznlGPywyM2xg3dgIgBNf8BEVtd2biJ4hjfbh5OneMTXRn4FYNViY30YDi31wqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM7TK25QDzpuDtng3CrcA%2FBg%2FPlijp2fFBeEGt%2BDG76tM8mo9sHucJjpfoVYHp1rbMOO4CjpZ%2B8Eu5dKthAFOKe1Af7ayoUd4f1z7Keb4mBpwkNiVUTRxMWFKMghtpAp2SdAjUorPwij%2BRi%2BkJgg9ymonvmlhyR6BImBG0778WgTEyuEplxK1aUBJSel3NZvpWLLCgWw%2BANlK7AekP02b19hRCR0EDVLHEKP7PW3sg5WaijBLbKTaq56Tqeuti2FYnQxWuMWSEkuPM56U23b9auvu3Ad9aZkhSHXKG5sYcbhcnWqC%2F7K%2FHKlGdHFFiV3BFTzPthVVDrhWOLdcHZnkA%2FnlRBe4b9YMQYB2pzEwr%2BpHoKfjdnIEccCy4zrClUx61D2a6xFCx3Hes8Ev93%2BF7zZkq6TjDNzex8Puv6nz39EpSvc%2BFt6UyTJuLdSKTSm5PY8%2FwPuFeMubaLTXh1yCiPB6imiIo2k2DyDeSigTyF8o97Py0Z2yemEOUjOJW%2FUkOKe1OHHT8vTeL8wKtX5Eie%2BSbRZpb%2B5zRMLTQn0mir6HJ98CIhuNDjYfnzMuopB825TraC2DEIpBt47IdQxqu4G7jYD2cNOKo9u6GMO538MTk%2BcFHEJ4w9eKRajhjMFIQPay1Q4HFbou1iXMILKh8gGOqUBcSB7NaI5mb4HuFcZ265zPHZIs1EewyJ5BPO%2B1VX2MOEQZUcWqN3sxLslxrVjgxgbTrGh4PvGsNAtaR97W%2Fp2k2NMymVHOsNYO1cwSlWxUPRjY0e7vCEK7UPZch5%2FT09SbndaH1Pi%2BGtE%2B6Pyz6nB9LuYks0NQg9s5y9AULxL2r787ia2EV5luW%2FWcbzUZtWZlWOjQZzntirgEcsWf1%2BrR4o5cZZV&X-Amz-Signature=a182b971cfab61e879d2075eaff21ced05935276f1d5f43bad3b9de5fdeedd18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

