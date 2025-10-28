---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BGC6BVM%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T170103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIE3FYaBGpQlNuOBptzO7YhelyICgOl%2BGmvLO27Rdd92AAiBM6aY8iuEW%2BEn9jYMrzzScLiGaqHtr6iIPDEs9xlAGMiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsev2Mt3uTHUXxoiTKtwDoA3Jm5LujxchwOjVR4WZbjn97jWPdr%2FxgLIfH6iV%2FWK%2FF5hYQW2bTpIA5J0TsXOzHBobyFz4VnHpGCIYMZ10nHQpKyJ6lGY1oqMi411VRwsThKkHfimLb8M6WNYyZGTb68BYHDOVT7iA9g0LBvbaZkziWuy4srhJ38VsV40pUcH2W4sPuulsAm5k1jgfOcXkFFU3pLc0fjdUWO1Ga9yRHWcCw1RCwa%2Fsrty%2Fh2Mmkfs8n%2BNbBnV17TdoUmznc8bBFJ82mOYqyDPiQCF8MNs2OvzIw%2BVYqS%2BAegjbQEVZ8JCRGlH8xRysRsijdfP6xMf9%2BPsSPfTPwnBEVyPSq1eyH6BIUil6odRR4DrPWWkz9sdMDr3Oq3QL0%2BL2B%2BxOtWcyZWueYA1J5xAU0nIA7V7%2FMgLbA5MvDOwbJCNJ4%2BQq%2FPHARXtLJ6iLOW2p9UMVhuD7g3CNFHxyFq3gVIoT%2BSke1g9lf2LtbC%2Fu4kfcKlDyNoboo%2B1pNZMJHzsT0o7lQt%2BHkP%2B%2Fg7pNN4fjiNNLewU0elfeYofg0%2BKPFiXjDZfHQ7MtagjUTDX0DFNmXBd7O9g7ZsVeDnskGWNidptl3y8dQ%2BlwoOmfYGv%2FE2WaocS2eO6e9aOo2fkgxOB%2BnY8wgNCDyAY6pgHxEem%2Fo0okU2bbzf86bfuclfkvzbcE3qxAIzwM9do%2FkJxerYTjFysE4gbh8m79nW4zWNDj8ZCAje5dP0TVaahthfMzTTQw6v9gqTfkWegS9hFUEg5aQpmTSDQO0kGbk16Up6n4Z%2BLAfB7%2B%2Bsk3nWY8ln9Aep%2FK2T9gA3dC2lCN35316ffVsI%2B4URtO8GJFz6vE%2B5TgHfSYnFScIGf2d%2BbugD2Vge7n&X-Amz-Signature=5a8d670b82aa956c08d4d79a1b1c4ab51f062abada7c7b74d7241b028870f4b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

