---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RO3VNHZB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcwTa0ksD3BikxjF2Bu2s9pLyNSkUo%2BMHXQphScdPL6wIgUWtnWL0GYhOdtRiQ6zGiFOi2XKvz6A4MfkD4iQv8nvUq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBl8MRJzBQhh8s2gqircA3A8ybI9JzpDV07uBeFiX2x%2BqOML8zI2o9Q9Mz50VnncxWA7pkGt6xqT2MJOsGRwS5bb3FdK9Zf8lRAi7hL2PSH3w7iGyQxO7TFxxVgaPiNZddRpedos897dy2sQk%2Bm9Bvo4eaHm5d54DIKuJf5SnxYwhqAo0wb40jhFcAxVdWUwW8aB0OBkl%2F%2Fvgzr0HSSKmKJxEzhp2r3G2kIPJIhVhVWcEdFqncuCw7%2Fqn2JIahdaUL4f76f02f8ISDljPARRhv3wVzXwlmJQtCScDxZk0UPFKwadwjFWK8Z9yry3Q3ff78QVi6VkcDOA5Y7%2BtOA8x1euKQ%2B7a5lRhdO6R3pSaFL0xC53USIGO8REzSluVsQHKkxnJS4vbQLuBjXN%2Fd%2BLTfEus4NLfHAhLHPWhxr2bkdjeNcp7HvlSbQpf4XLhi5NL9%2F75GJlCfP0vQaxUUJr6B21ETxH04UYdIW%2BHSviWvYdrjGJ8MCfWiSaJ%2BPrvEw6Wtrn%2BmcxP1j5H9mfA5Jz56YFZXxURAcu9p97%2FOu9wctmXPV2UAjFJYlwqWWgtA21Dc9SapF6V73jUtuc6sltROVPePA7UB2kEl5Vmmro3O5TT%2BycsveVcKLVqqLEU8t2iKKceOvU96FR1eMoMPK%2B98YGOqUB6cxFCDe9EJFHyHBoshEOB19qPi8jhy%2B7S%2Fhf7TTGW6D0NrJcZehrJajKB8VgIQrevY7RQu0%2Bnaetuajhv158H%2BKrDBITPO3Pk0K0C%2FrnOyZWugb7Pg4%2BvLSKwbamu%2BH36gXKbHcGOMKirylUfXhTKOX%2B80On7du7Yf7mvgwX0xdUxWOGSSiSEf5V7xMUjKhHP%2FqkYw7a%2BdcTnO4pD3KQC1rwDMLU&X-Amz-Signature=cfedff11718ee455f387eb2408e4ec48ea9a2e0c61a7bacefe8c19d9f7b4dc26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

