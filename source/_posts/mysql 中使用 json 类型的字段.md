---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMENVNYQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDjP1K6Pk7rVnbynBJE5dpEgfHiKxkXA4DswTtE2kZ2iwIgNyYF3lejb8hCGyy%2BNBG69qwRUvftM3q0V5bp4HFaA84q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDHDfZgGrhmy5I0I%2BCrcAwKGFuwjJxSkg2HHwEC4D9gGq8a%2Fo5b%2FvwWrlI5ta%2BOqnkL%2F238aSmbvq%2FgTG%2B5NwGOizG0w%2FxQ0Vhh1E%2BF7%2BqGuHxtBUE8aP2UbmzslnnjnILBdw5m%2FD8gL0OWy2GEJWmZDPNeSj94hQUbOUCvttAPEDkCHqxFdEe9whXaRvaH%2FIksYWCiI1LFZoHgrqJRsAZ4BhSg%2B%2Fik7hNGd3ozaHbpFvvz7IJzgNbUpoLo0LOFKHjbrh05YXm4xRhdt0AKVH%2BNy4VE0wTZwk6gyBH3vx4Gw30FNNV0Aj8N2wYzMa3xEydqoR7%2FW5Pa4EhLywiE1Rk6iYJCv10ZfuXPOpJhWIkFXB37jF9RITIBHX7f7rBcUB8V5A6DtVDYK9ehOKecaQuLsF1x6TuiDQG0ZfIGmfOeMjHYBpBnTaAGPOd80Ov%2B4gvsP0AjQcMaFavxtSVRCO011m5%2BiGKtdI72KKDMtBP7ii8VxkQlIZPRXZEfFv%2B9lZLKkkKQHg7GRFsqom0fIEaDng4vc%2BrMyvRr1eQax47tWA8ld8FAEq4XHSIr7d9QnAxapNuhfPD%2F4pkAF7%2BxPNIF5GBZys1SEnrNqW1tZTavED67e43agtymb4pbIBxijfMT%2BOR0dItWOJMc2MOemq8cGOqUBZ%2B0O1Glw75lCgCp%2B1dt1fRs5LzipMyUAG0kDvKXdHhCH6GAF3%2FucDmZj%2B0nPITfWebIwWTY7pbCC9BJz7HDq1I2Yjdb8l4GCInuhql%2B1VVYz8ldTh9vRD4lDv5wMvcD4oRMvQjAd%2B%2FSvomeTCTOJPeHkz%2FLMWy09t1YTQbmFmkGN%2FtOR5dlPQydUcMEDKNJHG%2BvrVC9U30B3ADdWLF6kWfPgvbeZ&X-Amz-Signature=0c3ab3118e68fd909e8d01e900646b6e58383a12dce5387001fcd28730b69b4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

