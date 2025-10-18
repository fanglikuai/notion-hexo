---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MIMEBE5%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIQDn4FRP8OrNS45ylXLTKcsr5eNFi6yHvzfWMheU89%2B1yAIgBP4IBoOpsfG7ghxXXcrKybCpbbB2O2NxlgwTUUou8lkqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAVNLZCP1wFg6VMGjyrcAzaM5HnUtGs%2BqBRTPQg1M153swGlclEZJiUm3vaDtRHl1Ki5C6aUia1kQNzqVAPSXWB9aGxcCfIE%2FvWbdjIX3buMFiObBGq2nqMIrWNHw40XJs8PwfrHHT%2FH4m43TYP2tiQ1dGEf4McMdclTektxWR0KeqAZHDlWltRae%2BYxIpQRI3HM1UgY693%2FeypOT56fq8Q6Z751rzkTf0QS88twANxBUfXtHN10QLz7LbqK3kCYmKjhXJTXnGnXPU3WmtVyRKOQcWb1wyRkVUMGPkcTFaF6rfNKSdRIaIJIaTycBgUwlSUm2GPmG%2FZwlMo48ajAI0RjMhQmbitDec94DIDq%2BeXVBtNxQD7XCNJlfG2YekxpfZmOXOSyDNqoRODTBIm7Ls1VTw3wHZZfg39REN4sVvj0zakhg2wxm0nt8JTAEsDzWMG6exc8RU4KP2%2Fd8G4Tbeii%2BfZAkYa1hBAdUIy1Z%2B6xuaSvxOkdTK1lkwjjj6Kv%2Fza30rMkANpxyccJNpzG8L8axqQy1VcET8etwuRAR%2BZ16dAV6e8ycZfVr17K3rkjoIj3cBTe9j1QTA6egxEDgXxGtEi7jNUwz3JI8q2ERtPqz2BA5bTtrQBwKAXz7lt4IUIb4uvIhqLkboHIMNKCzMcGOqUBuDtAzW0xx9MJlCsA1vJy0qCqbB1XBT10fPHAoOR50P5rilvXfNhCQGsz6hqiDvPzaphzywRxsNKS5pBSnx5RiSLrMJcPXuvTOeEY9LQ4sYv1H48j7ui%2FKqqZYNBBk1B%2F%2BidJF1Ezia4GsNqVxQxuq6p3oq7bBLZiV1aeQPBQiBGEA33rtVMTrijO2hbJW589y%2Fu%2BjSn8RE2SNT9EfzkvjOwS5oFK&X-Amz-Signature=2724717303654dbe882609674f8ce0b9fa23b2ba4a61d08b5e9c1a443d5e5885&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

