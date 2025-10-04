---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BHF7DD5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDISBXiPGPzMoTyh1fRW7TzVqKN75BJZsV6U5wdh90fNAiAhmtxumBgLzpdW%2FEObuS%2Btm%2BzvOY6SExD4tYiwGUWPOSr%2FAwhnEAAaDDYzNzQyMzE4MzgwNSIMkVSRsv%2BiIvdah6TNKtwD2ixj9tPk%2BYYf2dwa%2F9vDHAXhE%2BSx1RRXLralb5mvr%2FdsRDkXTywd21h4zh4z5CLyCcujs0fg9dBS3QLoejMmQCaIlhoZ9wzwi%2F6poeZpTQs%2FQ5rzMG%2FZHKCaYrIt%2ByWbu1gPoAQff6KP5Jmrv%2BIIeQDJMxQJBpTotyrfNsUZmCQjvPMlZxM%2BE1wkimuu%2B9smgOE4SfBoO7rhpPfe50Gtm8mp%2FR5%2BbVmJByGvetNgQHFRQ9f2KhMRu%2F0ZWkndaChTGD7jHLI%2Bfgy%2F9B7ZcLC2a8aVt%2F%2BtL8Sw0ymjsPj3uhbZZnB7%2Bb4%2BBWMUcql%2FU4QKAFLL0aWEBWPCC2wsZ10wQeYt7VUevSGYPqHh31CQgkBO7y8talfdVA9K%2BaF1i1mpsUiNyuC%2BPpt519Eny6uE7uR0jDm0X3nVFbaQF%2FEuDfdODib6%2FAv23TRhvA2M03PTdeuo4eUNlA8vO1Ozuk9iO2qJ9xE4M%2F6HLNpDG7gbafDLEjMIB4Px77c1kSfdtBTODvVHzYFJSmsposOdWK4m2%2FRAqsb%2BRpAprZor4rwLULQriHXFGoogOOq4VZmFTZuhJeOLrI9Rec0cq4xaraZdYZTaTHLvz0Us4yInyFL5iOO3i8KlZPhkM1UAmhEwvrGGxwY6pgFRCFuMYILBlVXehvgaTViJo%2FGdnnC9Qw%2FGMpQ0bUJi27QWjTZHm3ZhqXXV502CMgeS6PaXIk4IrPCO2OXzIck4IWyzChXZr67BvA8aaC6bf48GenLe54UdXJiohJnoepb43Ari368hD2eBjz4pnJlfo2%2FHmnr7W2Kz%2BxwacX8ykFMgVFATFXZr18YyARjhzUylc8Wduq8ZWqzR%2B%2Fm4W8%2BgzihffZEZ&X-Amz-Signature=f1e6e4c865ad50bc8c6c869567dcb8503ccb0ca90ba2d343a8a5cb1fcad67ec3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

