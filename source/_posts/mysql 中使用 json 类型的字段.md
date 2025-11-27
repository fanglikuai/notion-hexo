---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKFDOVZL%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNvMK851TxZCAoHRCiFp0jFJak6g%2FXY3c8MdcgAYvHSAiEAxSjJ6%2BoXd8Ya8W0zMWJn6to1BGE8L3gP4WShqqbLbpkqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPgEnhApzMQ8bVUg1ircA5ApVTX4eLcYTtl9yiGV2lzytRTeLuVB2guybVgPGybPpAvjuDvyfJcRpQAm0P5%2FiylQ4kXQcUrbDpLOHl60RewrlfBLi57Xz90hfl1wo5h03X3ZSks21befhP5YRdzaogxX8UkZKs3F9W4mBDhuNgC%2F1Aj3DfpmnjhYmveQ6KorkUa%2FqZrwcyHJyD9fpvlH0bRy0738pDiTE9ZiEEk3pJfFzXtm%2FcqXscU%2B9c7Ss594%2BfyJFs6tADRMn%2BnHZeAwxbu0ReMWiK0mle3426TRBOKXljNcnvLa6RBMmUwrHbPHdVek%2F7iXnQ8Mpq49lkkAwnVOHf4iby4QT4SetCi%2F8sjWWOKsblE1OMtCq0QjJE0eFheT2nHO8jMEe5sg8%2FB28%2F0%2FhnarLIGachKj1hGe3JP34%2Bg1hzkLsp8A1Kne8sX8hZJ%2FM83hPWCofoQpACCk2YiPN1MdJgRoDy3fJO1z%2BJf80SBE4gLUJhGMzEUfmpAYvXUrOwIMhfEBqWsT5rh%2FzmEbe1hma4FqyTnuQ1yA6o4VzTDGSil7nYRmaI8a70KjoGrPE%2BRjhZ5g%2FNqhgjzNfLqvWAjyQwyloCsqL2J6HGHfeMIdnV40T6C4QHvTMXZsUWIO8nXgrf2Uj2sYML3Zn8kGOqUB%2BHPWEe47LCb9CF60OggtorHqQKHv1ug6%2FN2p17layCfsFt3yBSzzITCOr64qNnZ52SDNMmA3e6SAaHuoc0ljMyIFahMv1qgFnblslHSVOj8SV6IFVydNO3CpAt%2BlGasP8UEVGHK1DRK36ntn00n6ErBg0MHiGKQ5We63PsWIAilqza73WiiVlKLD6fujyE%2FQIu7HIV7fyX2No14OmJuVg9uFXIJe&X-Amz-Signature=b1428dda2c6543ceee829924ef40246080301ce210615ded855e0885920680f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

