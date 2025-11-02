---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VCDJGOU%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAzLArQ15PfjLFVE2cCZU%2BvnxTb3Xfl%2FLoFOATxiA3ewIgBhykf5wrZef7%2BwPQy341fDXAjK4eNdzHCEFaktG6%2Fkoq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDARmG8AmPJE1mHQXFCrcAzSDURgOLJup5JoixQXkw%2FHi58rMu9bgdvbXAOsMxu1ndE5DukGyYL7ggUfCoIixvMyNooLsLJp9HhSl2xA32vrxcgJBE6xQ8cUJ1lwmJ8BEhUw6rAlqS1gHokO%2BPwiN49O%2BBK89y1fL27VxM%2BhasBgBywrFXscjs1KBtGKDh8kX%2Fu%2F1iRV2yGi9%2BQwTQJOYEDK71CNyDDGra4AoCveQafORkxb8bMB5T1PpmpP1v0UBtXvQmQTq3i9JSJmrvnTqmZqH7V0cQDkSFi9Ayg%2F%2FTwqeTMpyq9WAqn6m7nThFUBCFBspXxw%2BUAtFtvwVowklA1oLSyjDcrjrKIiZpVgbe4adqRYq%2BmM%2FNtjWQZ2Upjzxoz%2B8zJxmqImc%2BwtogX6XkdHskf7Wkc9NfAonpFALaMuaxhU%2F4p2iQD9ueN4P3UymHZZsZW4i1Y7Qujm9IyO%2FRZqPULCPVl%2F0BB98hD1Dq%2BsalXu1Rt0fekvAKeZ70uV4s%2Btk%2FMtUGEtjXKZygHjLIyvOO07DqAfSZZGQ9z8c%2FeJIy6UiD%2FtfPpPNNDWk%2BIOZYD7tBtervFrCbsR5OfCETaIXLWQlMJ0SwAzfloJf4Nas%2FYnvGkfTFfNAohdTkmslPx8OGppYqj8OI6T3MKe7n8gGOqUBshIg%2BRnRVzRsZL7CAbAdx3tHT89H5cA3Y1tsWLOd5789f6JM54IcckRLWB9g3D5viFe47nzye7tarA58fVB7Iyv2po7nmzGXjKwBXdyd8sgXkgS4Set0GOe52EQ4%2BNepm5WUBvNg54F27wCPSIvlxqb1lDHVCPjqZNn0l9SJOYFn9PUDUhRo5XChir1Q09FofJuFyu5cesArbWiXOXrwjgB7WA2I&X-Amz-Signature=678eb25d0c57f1ddda68a0482e20088251c9d3ba7c5193899054003fc3bbaf87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

