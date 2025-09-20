---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGBQYZSO%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIQD%2BpEF0wwQ3oSA%2FV9JfXQJyI96hm%2BGXreR6UubGD8aEXAIga%2BBV14aIdyhexOmOe0qJNTspowhExhLwnsxoy8uy7icqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEQlEB1MEnYzupe9SrcA%2BQoLpL4lwpdqaLGGwwuodkKUr343N7DAV48jMcXpniI%2BA66iHTBw75ZCFnwWxJHK0pkouodmGbXh5NiPPyJpTdYpircGWU6%2Ba9mLIDZmNXkENyZyfaxZBu88RFdXMI0THQiDIAfPIVF1ZB%2BshITNYp3zzLTcP2SpQLT5s7zCtmzSpJIxqE4wHOKBm00TENpYcWi26yoRBSCeObs3rdMNBNVLT1OEkt9WB0YnCrqCrM%2BPuaQdsijKEWyWfpg%2FEUgzaWn1I7LzYRWjHQ%2FP5qF7xymjANdFoGlNRVJmYHyXu%2F%2BIQVmZ5CA3Q%2Bh8EzTalD6FpDbXGYFiEpBS%2B4dAqeAAfwb7iVo%2FWSn%2BGA2I%2FiCB%2Bd6%2BxijWXjWP0HTPFuEdsgvj%2F0auPiExagtjPZC%2FlfPXtoo1o4M6MFX4a5K0gxlCAZvnXXsY1AM1VlPsadWA4%2B0E6ZXnjyHKFJmv6flA3np3h2whgba1GjsPH6cFVf%2Bv2m01%2Feae4V6rB%2B0mLCrqEtCqLnByBH0dNXVCe5adywDNriC8ThgzxXseBFObq7cTD%2BM0%2BdSnzyXUDaTzE5vOGM44cE7QzkdP0IBqBjzqE0HaMPRpom2aCZthki5kJ7oeilz1moe3zJpkFOnd1GfMNHbu8YGOqUBrAY3LHoybVA1pbZ%2BPHT7NqxuwpCOVh0Y33Y1bTcTeFfrSwiJw0QEAwS1kgwAfniGEBfZTOwmmmQHgBjoqSJWTJMOaSyD4K5H6NhNpW%2F3HyAU7nIlTeoi239LCBQdIx4yP9sRTpP09hxXUOFocRw8jRcBk80yCI0PrZ34BBd8IRtfuSWgYWRoOcnzPFC7BZd4wXyd6HDTgCtGGKI31XdhOkmmnxwv&X-Amz-Signature=20120f9b49dc76069e87edb7e4e0c122c4236827e84798f4d2a2a0010a2fa41c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

