---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MCMKTKO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIGsBUnJjE6kMYAgltCfDkf6vf9GRemXVM8ElCDi1m8nAAiEA2JT99P4GN6vElkz1oGslhf%2F3ve3vekI8w7Hbs0Fs%2Fhsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDGkxngAW0zQUbrmdsyrcA6XD1RVPey%2FR8DXKtZIm705%2FBuat3TGo%2FcG2punQbQGRmy28nfSNLOfCCVU2BAP%2F5cGNEj5MAEr1VAfvmV86AvsA0kXw1ggp2VHH3I0A%2Bv3Fu3biqEk44gg4aGcz7d3UXXeUda2t29qrUADLagTqIn6FkX8BiB0NYJlsJzXPIbnr%2FILCy387FwGk4gNu%2BzNLO2i%2Bzvb0xY5WYWHsgXI6fooHjtIMic97D7TyVnrsn0Avk5A8uG%2BFsvN0HV0MMUqMUmGsaV9yrbm1BPAKBuBmfFzz2pxFlmrbRPs1Q70e5LIcAp1iizqiCcVp%2BPh3TG9apphoZQS45VJOAKBXng6HaiXMDb7UiHtsRCB6yRhFZQ5r4zKNa0F6%2FChcZErX%2FpjsNDDSxHE0xFf43z%2FDrr8K%2FB%2FRtVDmUym2gVgyomAQv0acvL3mfQu5KOYgdgUXs2G2QHZ1snDUM9DvYrZkWiJ%2BU8bjX5Kdn5qb%2BLvQ41WZdnHT2MElxALXpPTkkrMtp6dWq3E2aMsJ2HEzKoMT5Bz9XC7VvXKGbGibF8%2FpKuYXPOzP5Pemp1x4ypnZaCa4nJbY0JY1nbfVzJ17%2FZP2w6Jpu0fd3hKYQ8EdXC9cISpWBikt%2BUPycZBbozbaHdXmMJ%2BlqccGOqUBiNW%2FtuqVdzISyjgXzG%2BYMWsvi%2Bn8t9EYn28Gl29vM41y9w9gOMUpBZO4c0HUhhP4ZHrMLGF0uO%2FqVVU8oq8Q654KzLDbA%2BTm59ng7d%2FWZS4wdjEVbQ3gHnJwZCTIvUIvchV0yvQ4x21DW74tsSBryO6i3fcFgXewZX5eGDInFMCtjTQdBaxkiKTndRaUfUslSZLVs30hU%2Fo1Fqeh7shEJYpaxmrH&X-Amz-Signature=9419724f19271c229abffd80d947330185647f2692fcdae25aca4b673114b299&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

