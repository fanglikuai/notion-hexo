---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZUQNRA6A%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIHsBZI%2FdWcLVmm4sDbrASprF03DDJj3gv%2FpY7Jd16rxjAiAk57qt3nlBspL9GkGgzZnkiuD2ZSAqn3l03E%2BEb9Ib7SqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMb7tnqysV4Lar%2BOKqKtwDLtjY5ufDAMFD6PVDLG6pZAIc%2B2UqC4Rmo9%2Fu9ayZDl22bfz4VdBo6KfX2BqxqzSH8x59XhIH7GNWnsO4VV%2FTLpbB2whKMxdmdZB3UHh3g3oydcEw0EDE3o8JO4vkKQkOcrP4qIGGqgXA79xL2204zHqWtX16aNlOY8iADWg94MetcCdAd33VRCYg1AZDY7oHy0fbik7Sws96mIUegQAtlpPvZ5zKKKuud2KlUUZamJMAQ5vtax5ZkpvEhj5IqJHdU8ByuOtDbCT%2BhkUH2wCytarQA%2FohWxhwQ0EVlMb3kJ4dBLo5zVvtQwjBHX8hPB0ovHow9Wg4gpWnIs40ZXtozQtyuNAWaIcIWyzLebbdaoNu9LWywXh73CL974wXf1vGDaQ7EtCPzTZsWZJmdU%2BOGw7Vv0kq%2BQJXJJ1DSVK%2F5JlazZwhaJ9yrfpLIW70N6i7IywrHvbdPyVWGF01MljY4G8L8fnrSVrRujZeHTz9Io76FtQFVFJeGgtt5Ga4ua%2F5pf5vNRPv57Qtlo8ypw0Jh57rkxQW9W7MNxB393qhmvh3h0XQ2kZY75EwBEYgedo4dN1OsX46odgy7O6sAVncVoUY0Plo0CjC9RWcITK6fwg8BnwHnUh1jvCONAIw2empxgY6pgEH3bwGL81i6n9233sC4NxXp2eHmdplnBp5mrGYWxs7wEkxiZURfVUhaceDc4cpY0HcHzmH024kUjFycVadAQ7tLdWblMqxtiat2b1hiTEUoHu19Jd2vvDgc00xJeiDrN4%2ByheeT8rODZ0sGWv1UkaEkqYelCvlcnFmA6dWfDtLI2vnoC3ROh0QaE4N5Ah%2BQ4sO00mKgv3QcCPotwuQ3uSEKYTTaR9E&X-Amz-Signature=8a596fb11d5bb9a12f076411e3cf9f70e4ec9aecb36302612827d564ca6395f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

