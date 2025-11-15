---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ONPCSGH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfMOE2OKz4RlwVabLr1OP%2F%2BG1vnFkAsd3T9b2RQcdIXwIgEl9VL3SQxcqtZ7Yae1Enzlw%2F3uPUCW2g%2F8aKPg5hZwYq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDAmRxRk3g68qRU2t9CrcA%2BVpMsnlbsdzh6FbVVNuzERkm8nY7hy%2FJ5y937l9VrtTAJGQ9TvuKkGlAj42iRuX4OUFbwyEOb2E%2FafSzzN456e9RnSpYj9w%2BKT%2BgG45t3p3ld37fMxMRJcBp4uakuZ8KleQUQ9RhSgfTilr0PiSyaSEGqT4uVqpFxOpKvgwIo%2FFo4ek2msCHzI3BZFDHKc5yFHVYXQ%2FZmdyvbZ2BRd1PuRjBvO0ntpums2p1aMVPUqDc%2B%2FJ7EbE6H%2Fb%2FhYnJjX53pjrSOm9Q6Mg9c0YYwJ5dDNdcR%2F7%2FxDJ3%2BcCBi0Rf%2BFwsjYHaGETSE61pnAx8BHIJm2jwvMAiQeIJP8ekFNB6jvVD%2Bi0HuH0hnIPqW91XPVS09Gtbc9eVYkkAtpKa%2F6ohu7Yqdu3gP1bQ2vo1ykvWUe5aVjOMrVibhdnD8fTYaAapJAYPfjgSPZEO1bIlXCNdEzCZG6AkJdDirJKtwbnmjNC84oJKlMooMHt%2BLLS8Dzr1s%2F34iW9hUa2AxduRAYv7ivgQ6D9jc0GDUVmf7TrTkaRRUnM4ShiKqPvi%2BI3QrqhQ2DqBmOiIBL%2BcLcl2wK8x7sb2aoRoazjJvNqbJA%2Blz9ff8f1cQms%2BSQVyI%2FhsfQ3d1M3fjJp2QKNqIcQMNOE4MgGOqUBVD1YxENr28LfrMm6VHhhKUO%2Fg0xuXIqRsedbd26qGyOzi77VY%2Fq4qEl2%2BK3ZctcEicj7h7HMZV8xPnQZ8geQ86emdPW8PNXkQBxKcXnOZdZLy5buNDipcSOYX05NOlYOZZBv0CvzcFxFXb9sAPNlKi%2Ba2D1QgBexEPBnwqwnXXSYxijrVtGXD28oEhVAN%2Buh0Vh1OjoJwu8EMgkzf36uAvBd6i%2By&X-Amz-Signature=9c604ce8328fe9e3fce664806a3252062dcc4ce9572fd47837645c54ffec8398&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

