---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKQAHMOR%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBfcTl6vpZ%2FAjx2NxNGWHqyORGNFRgKNIL1L0xThZuNAIgASScDceXImv7Zuqo4NUqJp1K0j1x0hJNdDrSjBSmaC4q%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDPsl93u9Gv6wMggBYCrcAz7kMmyd673LEl5MCJhMpvckBqWnr38R3QnVj8qsyWi0RhZaaXYgMyu76tx6q57p%2FEF2PU%2BhQvesG3NyE5xuMLReFUNvipvlfmVoEAjC0dEJLi2DZxAN%2BO81Vb80M8VPME71P6tA1DKifrbz6eF%2BOUhGy9Ct%2BKMXAfCLLgM7f%2Bj5YI8I1gmZFPKspEWZYZopI7F4sdKXTLEoouHOhckkVZooNyxyD%2FbwKTBNgcUKGvYxNTWf92CHs6j7NR3OJp20j6T0TYMkV2eHQ4GfGMbQU5NzqZR9c%2FmRJKH%2BitkgJCcvY9yrvK8jSX0RC%2FngSUMSuv6kSOa%2BFWdq4kSyAIc3xEcdAY%2BPd2pupgC0w1FbIM8xjLzNvPw%2FjsnQccB%2FEJfFnJ3iAOAoDYaAdJxswi3PydmxVVNnOe6DmFKQWddq61ZGF%2FHWWWkHig1QzIsCZ0oZCESP%2B8doO357Bf%2FH6p1tw6nxxXWIqbZGuyXwMBZVMORxRv2r2xe%2FWJFuKnoyyXUji3mA4QwoACZmLP002keBCuAXt2qTHCB8mN8RwRzQnWMc21%2BBgkv2kmNBtPZZenXRaHhHxro2F1SAKixQYist4boufapiSeyxGnCaztLDbYWCSx5NyshUQ1kdCg8YMLXnzMYGOqUB%2F5IAkJIIenFyRhpj9IAUVuV%2BXB9CaGdj%2B6P0%2BaxWKKaw9L%2BnYtCWYvQHBiPk2Pe7kRlAUjIIKAC%2FyBK97I2NP6wB0cnRVqsXbwZzj02xtBf%2Ba0ymjdGmErecJiUOZSHgJ37TLWmI7vdHry2gYZsEzXAviL6RCfBvZEVqZnZuMaUkrJRWNrzSSCluqdjJuIYErhAmwOSlAWlnoJ%2BjrejbW0Y%2FHoTu&X-Amz-Signature=cc04e87284489c9bed971fdb074b19b25aaff66e5728ffb667ecdb78cc7927ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

