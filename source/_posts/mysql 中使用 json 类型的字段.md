---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT6G3Q7D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEG%2B7y%2FoP22FMK76TCfb7obGSEjDw6cfJcB7qb%2FvU7kRAiEA4Tldhswlz7olPCQPN%2FiVPz3yMWVcC87%2FHkHXkYUV208qiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH1xiNqHn82DjathsircA1JzVnph9ZJdR0CgPnV%2FEXQLb7GSMFFzLpAMLLkb54OnN7AfMxawPzSJaKTtFVjPY3c%2Fep%2BG7UQy5%2BDT4%2Fs%2FW7puCJCxF1z36aF3InaHgTqEJNRz3rZH326RQQvMjQ8p4gUeF3R07VjsK0WJ%2BQ%2FURwKa6%2Ba%2FxltGfpKsmRJsA5gSXGjHjzrhmwBV7gYRe9vMxhtiT1%2FYuSUj%2F4YGk9MaKQpbAINox8eXkRwtLsxztCqiyPxFcUbzUdRXtW0MqqwM%2FlF1dwUyteHhjBOPpWDrYYARB5TX%2BVORjceFNprmLerhZg9Uq9x%2FmqUc74pGih%2F7PIunAZdaGIuirycz0GZB%2BwRkU4iZ7QRmlYu%2BWiktH7jfSr5fX3Pbu%2FS6Udg892lro9IlCWFsUzTSk8aqy59WE17pvwz2CWhHIOEHre%2BQdKPeQDYSOXG9BWrHVNXOobhrnCc46fH9Z6hgx7HAm6NqkunWECHo1ywYxEtSyPQn5QMfdxfUs8EjucuLcXG6zHCeZr9yIlEcWd4Fi9tQtZqQWkoApwmiVqYeWctuWtUvgACP4GygO3dX40NoLLHJ4u0jETRiKZwB5Iq7yrrOoAWptfmZqLL3PJ6w4ClfmJfAkEkurhV0kmtOcuBgnd4aMMCktMgGOqUBGe1yMWdulZFe7%2FiVWnpYLMTtyeguQ%2BroKRS1E075dcOwDRKJhJ79qKmuuonk3dQjEnTN6QpvwaiLKiuv9iHP0%2Bo3e3hnUQJ0sAidRiagocXbxXsgbq9TKEwyAxQ%2BgJ1jU8h17tYYdzhx26NennFcLkLy0eSMyAhiGGbkEQo5Bs5HaGGcux5qmERbHHmC8xVz%2BtmiEcTfh64XnVozqLbgTowmLPCo&X-Amz-Signature=b5c21b5287c19f6db8a37242b63eae54681264219606582ab05e81d45a308373&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

