---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNBP7Z6I%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIE93b6eScioPXSIOTV95Vm1uxTO0YiGUjpDFvKLTFTmIAiEAn1VpM7mAAUt7770KndaeKFsDlAIsLlVHAWauoU3MUtAq%2FwMICBAAGgw2Mzc0MjMxODM4MDUiDLNQdfydCv8KeNj32CrcA7FFGo4Ra7kNKCgxiZD6G%2BiNZ6pfqNS2ZyVpyDoMmUNz4jB%2BRCAG%2F7SQ48UqZWpG%2F2QEzPTPCRCnn%2F51DUr1wyc%2Fes%2FptMD0W0M8CMv7KrsBS34D41H7Cvw%2FVWbvWqHHg%2B7SeRKz4bmVm6zop9d1sLWXcajsJyqpe40OlaxE3hkydCw2jraRNpVYNJaPvZ0UBuCyNdJ6y5zxc2KzCh3RgAmpgwmvkQ%2BHwDCap7FneDDa1H6rj7UFc3zBM7HhKfmXpMbe8gDlF3%2F%2BsNx%2FoauHGK43ZdYIECdvFMyF6rR5wfLQO2zE0jd5dX8tiRH0gK6R46J%2FNFd0zoAdGuJbKgYbYOOaJpG%2FVdcK37Sf%2FBNrKcGEWygAkctojBNi%2FGwZ6GeiuN3rAUAnjtzVfdPtYwyEUh7Gu8QyCvRRscFevtL2YqapvsKEmajmM1Or9auCZNSK45d%2FdyT%2Baxbs04wkVLYDxE0F6Emu33ow0aj7mE4N8OIVhnXai52lzcq%2BR%2FT6gvQBJsDc6EBGtlSOxBO0Kt5W14xftBh90%2FvljuzI%2B678yE1YDOEu1mf1bDVM5a9AJx2MeVgpy4Fj4ZKw%2FRaRuwpa8%2BdsN89KYdn5oecAB7JFJWrFXtrZWyvFXoBBIzxYMK6fgMkGOqUBuLCN%2FZkZrqZWDj8r2kjw4%2F%2BZKGhDhR%2FH4QVp1CFZB6dDe60r%2FqSfH%2B%2FcbHIfpt%2BjqVWfw4NI24M%2Ft0QKjw63O0N10m6gyRIXnNZUVRGgR7E7nmulzdjNwebnVfvGju5DEAFhICo%2Fe6MQ32M4XIuOqrcsZ61EhBIvU3QAOn60WoTHEbueOly9uFRs9ErLhoIK7SWWWsIQbviY7pOMu4FZbK2WF0j6&X-Amz-Signature=8b8592b046f69939bdf1d4ba1bf8919c91525891ed956ac4b3172014c982bcc7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

