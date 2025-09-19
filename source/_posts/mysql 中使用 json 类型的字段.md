---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWATQHQB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCGeX5lf7WYIiclP2PaDT%2FCoUOoQfHY%2F%2BHHjvk7d31p1QIgHEsXtm%2BBm2HLJ%2FfPtlL086CTtkPU5wgWeneApA2Zo3wqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEAcasX%2Ftnc4OPExQCrcA8anrEcM9imVEZUpFk7AcDD70xEwdqSDNCzqiaV%2BwcxsZO5Ul2Exx7saPrlQhM4maQNe6tIDSiloBLOcMUIF0dOsgG2pFEGuEic5T4bfaJZZyhdO1E0kt7pMQKFKSUlU81rghJTaP8I3cHRNbG%2BM9AVFqAi%2FCBBC9ulE13I3oJdSyhlQ1fBPfAwLzfM617dtEYiNhcUQO7k39eTpdUUEQM%2F8bHeT4o975KxvXM4%2B3D3m29z79IXoPEBlP4KNxLgFQxIdkx3LNZ3LOW5qb6CaVQszD%2F5iXl2rmVB1f%2B5SEcsJiWN%2BQvJsHNg%2B4XTsXmZwKr2GfKa8ps4Frc4%2FwlpDWcHfakgnPJpGP160pxdFaJUUCDD0T9ZVTVfbbx1w6Jy%2BUnZWf2UXWPiasS3rvf2BsoIUY9AOuM1DIFuwFzYGBS0kEWVHD1u2kzoym2461YeML3xJjCuLLBUQuMXvu%2Fibx9zU4mK6GRpRUnDBnv%2F5p377bk%2BAfBn2262hqVcCvjDeTzCDQg3ZrgSynqurCY%2FI3FpmjhuYSdYKBq7CsjQpoHJZrVMcWa3qe%2Bz%2FvoV0Y4yEScLmTDHevt%2FWj3%2FU4NKzqQdWkPLGn78wygTWSpI%2BP8dYOeAUJFk2EE97fYh1MK%2B9s8YGOqUBK7c%2BSP2j71v0Wz0l%2Bka1P%2FhcYWUz7hHpw1SBm1o2d%2Ftb8ixpYT8r%2BCpAjd7JxszwvBmsRcC871U2kwh%2B9T2N7pS0YU5TcmQQvfwsp1Kg5FR4YUn34NZ1XHc6PNiMJpKC4eGpIxEFJ3APOSLPu7Mqdyd9HO5semT969Y0KVm6jk%2BfX%2FznQUFsD41ooYo%2BKJOch%2FY5CDkGaP%2BWTli%2BhQ2OiVKGADp5&X-Amz-Signature=6f0ff491507a4bf8ef5deeafbce1677c86db17b42b8bb952e473a986fe13e222&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

