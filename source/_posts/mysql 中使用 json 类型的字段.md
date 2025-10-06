---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IQ5BJZ3%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXbtsJDuLQwiy2%2FRaKDmGap4L0yMDm2%2FJAz1BLR4ISygIgDd%2FQi%2FQ%2BVhFmifNR1h%2FyagK2W7z6%2Fuh5VoQjgOIGobwqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBy5myaPYGqLs%2BIF7ircA4roR54tHvo2DB64uaubpR7%2B9zIt3v8kf91Ls%2BLA7e68%2B06DCKzynhcF7D5zhPLvunplNekGVt%2FhnuL2ml80C6nqUZy2subRr5mSdfzPtfp%2BZMZkbj%2BdkWzqF8gY1zIz7kfBvtzUDwSXQcYvsNaqifEVJa1zQghEvtSAsPkSVzsNB%2BPZ71c8G9C%2FWXQRSzSG4Rv9vXdln7yfDBooPMu7fVd8ng10VVj1OdZj1QoA4qD38wWmk1ez%2BZTUgRbbssUMwF3yNfxLAajydCWjbfOSi12Z0DcllMeJAI6jQu%2BHmp5sPlxH8JePtvCiELK1U7CZFOBbPC12Kgk%2BhArILF0P%2FX16ntHNwVUQ66IueB8nWC%2Bo%2F0w5BwTKgvOWTgO1u89UsYyGmm1wnnNOFCbxRdB%2FQIRJaFSmRHDtYHKQSWnPgCcK7TayLs82rKgtiYI8vIh8q3LzrKnG48g8ravVB03yFs8VoVdF9KEdkOrXAJ9WM%2BLzMrdGszbA%2ByXH4Z3Ms8I53b%2Bq8BiRxE0q6DDTwOhXHQFFcSpud3ESc6AtLzm7qLkBCMK%2FKym%2BEdDF0pj%2FJSzvdATxHDdSBG%2FTNeheyqiwlT%2BR57wjS8DdR5yd1eH0AAATvL01uINZd9NflwjFMPmvjscGOqUBmCz9yI4wXmWx%2FfaKRdfxR%2B4d2V16yMtywA5v8TVdyMeY%2BbHtcw8EyqG7gA9Z5eWVO5ktePjld7TJEVGGMv4dOPHaSECw3fgCUZT6ef85nF4Htbk%2FohrIXwkN8%2FLTqoCufmXSjKcLtXUyONPO95ID8a4s1zpndwdASV3FYyZsw8q3L%2BsQwauKXWvhSJtYv1SDRWgv0qVWTrXe8pS6B6L5NiedQDoc&X-Amz-Signature=fc136f4b14b88bc27b7814911f8aa21495ac35149aa07d1c187916817e19b6e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

