---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LFTDYON%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T040037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDF%2BkHucLuOgC6FZE6Km2uYCnTK5H9TCVaQo8NRDJvRFgIgVx9jWPLyEqZcnBd%2B2aQe9lMSZm4l04CoNFIE%2B8JdyRYq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDN1mS3gLFyBFYPzEMyrcA3c0GcH8WPpeIkdTQ34%2F7M%2BSSaFpjSaf%2BoypPm7xkEN8ROxAqAUTsdhfqEtbsBcT4vImhJxrpfwJkKHvzTkEkqLIQfzXEqehEj00q42noKJnTOxjcPJ0hXtoKn67nbfjv75lofXT9UV7%2FwG17A2M%2F9oRAMfbj970IOfOGFsxFB0gfZOkwcFCat3tfZ7snQQ0a9DX9HfbubqHaJ1rdod6wvvZOvx7GHuhrOcoSFJA7N4M%2BULUiOFs0xhMlpDyPe%2FExGk%2FgJiUqYcjRiUP85wqdEFLWslZasCoutqAnYs3JpXfZ5QLa%2BDEHH6AAjIu%2FgnXc7SMyl5djMnWwE0eLThLS3k%2B9ncC18Ow%2FlvliepkHZz3wuD81xda7imG%2FhLjG3mlsj7TIM4Ba0yxiucF7GYkWpjM6D%2FNOW7negpsfh1BJhxGEVhlrXPtT3acoapqiMJwLL4ixTzDrxdcgQohAqaycbhBpl9CjS3tA%2B0HydNFzwGxnASiHCJKtKhuD3abFPSoNzA2EPff49aWqYhVpKiVHCx8rBJ2OdnUOmYD%2FFOKTBSsFjPPaneW3qVx3gyfpnS3C9Q6ZCMVBR3sBSg3a0LrWkAVuAikwLFbXk92To9gwDBum%2FDFRokqxQtUhU71MIPzn8gGOqUBL3J59GHtzD3RKHsoa8ZtUwKIBipiG19Jd22aXLcZE%2BHb0gJ8byHYQcDou%2BhmecCJdhfZoT4dgjvrheg4J135wq1lgqPZwTXHXIQZYrBZACOkybIo6QhVYZXEto5HX2povB89eTWTtvioOdXSG5yrOBRZsnxETw8inYCGPLBIeOT%2FIffMWdEAIxh1Zl1yxIygZ3Zgw%2FlcLmP52WH7J4bpsrf2Qh2P&X-Amz-Signature=4a01d83b40c5664ce39192e47e3d69e14098d4888b4024781b489ccb89556293&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

