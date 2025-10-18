---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSITP3OY%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIBNSlvk09gDKwJTWTIV1LcvyUsw4CNjlzc419nYYKXNrAiEAmVYBSWTKMUoLAbKvAmQAZuYbs3GS52GP0KJ4jLSQG0UqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHLZMB%2F%2Fj%2FLLYcanMSrcA7FXdNCTajlLyARQpIO5vTfI6eFmLC2wOcDfpCpuWZRKSaKsV14XAL63mTXYM%2FX5XGj3AgeFvIY%2B5W8qXFWRw9YvyWczS%2BTCFXQb0bCz9kyxzJrgShMxcIyAnpx0gSdIK0jvtIbQEWUrCoerzqA06JEdBMxWLgwFHmIU0zjSBuoHtVffLdYl6J2ntfl%2BBpKMd9MTuii73e1N%2BRLLdftgXMuSRrTsAaZ0aBZRggJu2e7%2BBNzm4dMjJcHkN3dv%2B5eGzGTKzVW7Mn6XZ4BpR9B710fD1r6s4xH1R9EvQA7WAYHzewd%2BIcUfjzhGdkHMvoCDpP87%2FAq9qdNfJuUIvIDEO8B5iLmxQTYa5KJIMABttIt5LetCcfqTgeESBYPg%2FTBg%2ByK2lbbFsHVTomjLYJklGl0T%2BdNx63BdRFzLFiuiZdiZ%2FoJlIw5X%2BgXSHiCc4iFgoRr6SNmEeUcx%2BSuBmU64GJpazBv%2FM%2FYgJ36JxnnnLLm9P64vTld8PFNJnUaSpiM2jg5P5ehOo945rGcS1bkAPRA%2BpyUubcy6xTQIXVr3Gbn0Spht1EkVcBtbgtSbpRO2oLydTN3Q0cBQ6liHlCmEX65Z%2FFgvzUzHdLAxF5V9%2BZ0wqTANx7KI%2FOsF7k7QMLSCzMcGOqUBT%2Bn0uOC7F7FbEXV3ob4TJAFx54XZ1ey0WzlyCLhhzqDdEfhJ%2FjUEcG83AlDHi2Ne%2F2ParijW5hwKppiQAFqLXSpHL05Dw%2Bm2JqbC8UIVX8NvEWTkEcrNEVRq36B1Vv0qSBcTkUfwuV1mfNwGcwn0iPcxJPp%2BUJV5VUJSCK%2BfYlhQkBFbDjIB77jC%2FKPpSjrc0CBKZ%2FzYpRf1rbOs3P88eupzF%2Bga&X-Amz-Signature=9beaab46099afc090c0802b9720c790801bc41e0095cf436a366c3164842768c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

