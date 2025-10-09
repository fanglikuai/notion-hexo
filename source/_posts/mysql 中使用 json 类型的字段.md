---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SQRAEA3%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDxq5AGuBUkJxeTCIseg%2BMvBfs%2FEWYR%2BJSmhWCF8p0YlwIgR7TFCSUl5n%2BhvQtsrrN8PgeUvVaPK1ivFnBBRIj25xAqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHPU6klr5goaZqylMyrcA1m5Uv0DGkexYSnTwM4yS9STSEEbWqMyooq174T%2FA%2FtOBBslS0ulckyxxdoLaJbqIfJi0R%2FezTNHOAh7sV3e%2FPtVxSyUZpPhYU6UA6tOITMXhr3KLFIPcsS%2BccKbwP6ZVkovKKStR8lXl5iOuvKyhpZifmwWjdYhVmSfDL1z6Mc%2Fkt8SjB0vY3y%2FB2Gr%2B5W1hjY03MKDKPPHdmvHXXY%2Fv3uRuOQJGzu4a6OzTxontmdFsbxGQgKTWAER0vM0NFgzcbPGG2bySNSOKn2ZPBeob03Dz3a3ELQ5fbljSnW8O1Tn%2F3w8Wd06CsFv10KoUU8KAZFtyEbjyxwcGMZMz0AhOTXX8WirJUNFk3tfwS7rSn1vPBwIe6jpnJohXg3kFwQmY%2B0H8vINA5cBU7EtCZPADhWPJLemes8Bzli1kkeeuHUqyt6Y49Km0djmPHLZ5T6mKAHVtWDMg4cAbfu7VvMH0gKs5pgJ06FzX9HIh1GdbYslx4b4EWCGz9n5ZYg%2FJqbRs3zo56Tn78DrRM7cq36EkvDOWSlUSSSeomZRaVPIuMYTdnO2yy%2BEXuRdceuQhpsjgBuRx8QBokXY2hvINB%2FScdoBED2EdOv84JyZx56KVkAQq7hMCOsLJYvC4y3LMNyRnscGOqUBAVwwgAz2%2BtntnSayRbHRzr98ze27Tjss%2B8zPXoNDDsAUxc1%2FVfX%2F6hSjUXbalavjqEBEU0UMxtO3IvkRHqv2c4Nre%2FweXa92VLy26RCBNjEPvUSlDS5aYyBZ31XDenAuwa5B60jjbmRrjRLcPdNhWJ61dwQPFLc8zC7FIpK1astWRTj0Cu1Mru%2BsEM7Fy8eIMgiWB53VvPA4Br76FJh7ZfemTjeb&X-Amz-Signature=b64984f6914ad97f1ceafa679d6c497da15fb38cb33070d4eea1c257b590286d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

