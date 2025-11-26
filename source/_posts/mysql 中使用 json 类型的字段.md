---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GZATYLI%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T040054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDtos9SYJP4ZxUGU%2FcDzh6lVIyHvUepfzbHdrXXiaQAiAiAlPyE%2Bs5C7cUpGcrrSsVkhzTS2mqHIra1KZwfKfY1mgSr%2FAwh9EAAaDDYzNzQyMzE4MzgwNSIM1IDm6n0DoxsfMLawKtwDyHm%2Bb0zbo8K8SWyoYTpIS24E1M65FypwvlIeJOFEyk%2FZtspTFIT0R3sQ4KXPc1WQxoODHreiXD8ciCrBhbFZN4qojT2MQTYL8bIf5BSDt9NLymbt8B2qLoXr4z7AKs9eS4sYXuqfhaj6daYOd8i%2Fh50dzM7UdhqWwaJAZyuDX0Q3yUJ5pKJ5plTpFufvS1ilVkOjTmcqhhTrrDVUQMKcqUiTZYyCqjL26VCALvuHddg4s8HyXigLAqkQRezlRkRds%2BpEjza3nmONwW79Iz0NsoaqBff3NH2mGo%2FufGUCE%2BLHgUQf06i1QN8sdxa1%2BCmmDz2IeZzkyEInPZr9pnWIaRjjprAwXq8%2BKsP0Mrgg4XkRYmypirlikOxY45G%2FIRBhGdTQJ4aNZ3%2FMQPDYtKEiAr1YQvrSpK2oVI5SN1BwzEaHIupdd4T7MnuJhOWyShVOKxt3xbvCHh2VdZ3OTEoY6g6XEs5YakTr1P0Ir5KRrOkMXKb%2FIHjwn7iDtQGWejcuVbMnKQuLFnpib8px2OwK%2F7UUMImEvvd1B5%2Fh0H1ejNPUfDngQdRkiXiVliqRZ9KzPlxBB3aPWWU9sDll6fcTsZ4YZWMtxIuTSoduR62YwchTaGwEKtkrIUJeSVIw1uqZyQY6pgEn8nsu8RCb6hh8DnGdJby71DmSXnF5Wz9QJMYOWdhvmoT%2Fmm0bg4ftkA%2BRaQyBigUrnSsJye84MJyk1xydv2M0p2%2B1%2FZMq4YT%2BxbVPSO0R9hXhCwBsgiykZ4o9Us%2BSBj%2FUwZCayuj8%2F1bnHcO8BuMhVcTalgKI1iaxs7YYArRASfLyNw9IkrUsvkVWWif42FOVHMgXY%2FbpnCXje1OIdSlmSFUwG1Ub&X-Amz-Signature=686be3392c035082e79a859022e14b8c53259486d2496a7e75ec5047aad7fa46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

