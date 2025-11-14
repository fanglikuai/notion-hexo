---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMJ4IY7T%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAPr2HKg176zfwHkf0WLA6DhnfiJAsbPfgIflLWjG39%2BAiEAhIRKwW9pxW9ndKZlYDJY46f%2F9Q1vmE16VoTLlGjbnV0q%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDABOaxYkaSfJSsph4SrcAxrvqBVsqS68hhrjH8pLsJWWqa6MIr95x%2F3p8Fe74dV2BSz%2FZ5fAfprY1FlMzp0canXx%2Bo%2B4sjzWjD4Q3EE68YGbwGLMRCw95Vb2urZQylImSkSewHbprg0muMk4yztWZdlv84uCaLw0dddXLuYLNgxQ7LUVPMFnbYejecSl6D%2BG8T75HYuxWEeZ83v2t9UKJRiDFNxSNFsH38MFRXbCTq7cUegMVqbvpWGXFh7MPTdwdWRoMNwjXwbeXN57gNvjx%2F%2B%2BfLM%2BJB6mMaTWWMJ7g3x%2BUstNP%2FWPEjWxVzsIb7qdDTANnsCBO3fX3dFw6ykxuzNI10pBbQJZo%2Fp%2BvDk%2FXYgY37X0bUBS24Js2RuW%2ByyE2yYoThC1mHVFEuCyI4wjnMUOxBk7JaUXnDNbSjUG%2FKr64buSV8ZG75t7cQVi%2BBACs%2BswbWFDrpX2BRKZmzryavN6L4llMIdHrQsizRmDWC50slfiBgXPs147Iv%2B1EqwDk1ketjAd2EmnoVExm73%2F%2Fb9DvN6YieAhZuj3W5KfVfSHwcKQit3gBdkpHL3%2BFWS6yVwpan37KxoflbiFvbfBRlqUQyvtXvPuaKpZQ7%2FnwS4OOnhLzBGCVERO1qztxtpkiPM3MYC3K8PfDvvIMNyi28gGOqUBoQct4JeHXIg1xZ70KxkK1O7lMlg%2BJKN%2BQ%2FWOrQ1QITavAwYBZ1zgo8%2BVZbL8yoqnmWBCCJ4bSTEg9fc%2BdDYuW%2BSmjdWMYAG7R5J7OdCMATT%2BJym0EmWKbsQVa2LbMf69r7kucYAYNz4tIjamsSqFkU6W%2F4y09V4n7t%2FArLHUPaRJf%2FLVM83kssYG7Ok%2FGFGhEF8DgotsoGpSsM3TJ8kySuK8cI3L&X-Amz-Signature=a3d6e5f36cdfda88c17f81fb4e88cf3df64dfb913c7bd9603956acb6a16be644&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

