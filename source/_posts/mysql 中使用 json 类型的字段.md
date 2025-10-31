---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TON7P7LQ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIQDhNcG4J6Je%2FXJi5lcXTIhaK6uh%2Fxy25E6Dcx%2BGErJXdQIgDzIIlPw7VryKf%2FMm8wvjv31DMwceqDec2Y6UFqkkOFkq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDP6d4BIhGw3vXvIKTyrcA5JU8B8t8Mf7yUb1M2SPlPsGQv2kMRWiOZnzESHTsV8LpR6S%2FOgzWcoGuixbDF%2BCudkjwTmUu2ozBS3DmZdxZ%2FbrjOAfzj%2BgsYrTGOCUka9Ltc256T5fwndWAvclkuKHxAMjotcaNJuw4a5C2udrJkXJrGESOrwLpmKrloEQfB1WQHLOiSXn1JccsZVG5uGIpJjxB%2FW87Tw6dv3Ex0AOrOcIoUvmF%2BWrXRtegrfWO4RoHlrFCXadR8X3YUNuJ%2BsdGU8NvAS9kHmJyFB7Rb6JdMkX%2F3hhlf0NvrR1Qk%2BL5OvL5UgWnk7pZAQmsT7y4CHWAtoMTufyAJcQpvveZTtX2JNkW6E1ND4irTI2NuIZvO7zMh0omQI0qQSSODklnR183iTdwPe0Al8aO4n71M21tFJJckcGY6n4Nlf6RTggB4IHcKbaUi1o6MJFWRH65TwthoxZgF8fWvj3Vfd5ZdDoNRvC6U3p2487DLv%2FgXHQUw2D7vJTJ9HigNcymDd8l7Jqm8HkxiEQMMvPj8M1IYAXQDVz9GkodayarOIH4CY7rRYIAHrjBwSzoxa4okMSc%2FQAPTTq8N595ujCXO6%2FWsB98q7MAymyaUXzjc2eLR3FtKfuDTa18e24B%2Fnnqb6%2BMLbck8gGOqUBg0Xbze5gCfdbwHjfuLF9x3C4zxBM%2F%2B15gyMLp%2BLdP7Wzot3s84LITmI5piyGaokKLgy1dtv9I8yd5E6gGFO9AZhKt3C3PbTg6Hsyo41QtzGQ5aycdhlN2byrIdyZZyCKZhG5vHDjLhtb3mXH7QpX3g%2BJVEQBPp%2FClBzQdCh1%2FdJyD%2BpNWQAyuPmm3ETtL9h5cXQckS0JpdkNLtLDMp%2Fp7cNJTxrO&X-Amz-Signature=9e050ab92198b45ed70c7cb6e78158acb836f3d96e1945aca276d70c57d94b70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

