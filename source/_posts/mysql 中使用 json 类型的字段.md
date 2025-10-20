---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJS22WSP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T230052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCRakER4Z7M4MhdsfwPr%2FfcoqeOjruEU%2FfqDMJ3r34OHgIgBLwwAgmn6hxPgC8SAvyzkqEMl2yXUcHuA%2B6spnESVNAqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMOvtZgwARLIBkvqZCrcA7htZn0Epp1r5JpDXGgvQpcoooRLsU%2BYHsdvNwX6Obi3WVJM%2BYtxp8EihHrJBe4psoo8uBPnWieW3lb%2B78EU2mDgUwuD%2Fv0M2DshBkeTbeOkG7QVwa9HtYbh9JmHdSozA5VhghdKg3rj%2FfBWYJN6XQqgK6dAJ9TEO7y4ockD6XIuw%2BGkKyx3iZS0IFj93V%2F8ZKF98iJqVbptWPIicsLlXPcKs592UcW%2FLPhRFRq2jJe0SmxgE%2FXhyOTAq5%2B3FmigpnUDv2afWz6wpRIWN%2BXhY%2BP9iApkZeNwfMRvDgVtTjQqyG3MpsG1MPohTS6fldjx5Hn3RKaqhN7fPDbgGzn904KHXSubjzbHnquVcPVyy4jlKWs2sGp18l3AsuS0tc1tUhreikzwknv3yW1lx1T17mv5WKy102DbtySyO3NaKWGX6TQQtIjllXAiptYfk694pTXALjSkNcNtsLVo4S6mjA9BKFi3v0i6aZTHvKHTZt1%2BQYld9tnCxr%2B%2B8x2%2Flig%2Fyl3axb60iaNkg2GvJAehp09%2BMQ0tGxaO3UEFJGCjkIDppyigLyAlnjpRq90UDv9ETEpK8lGenEK2Wyphn%2B%2BNsp0Dq8wMS4rbxDYHkZ4Jp4GpAo9nMKIINpeWYeXkMPno2scGOqUBQgxwvl2rbOzirULFmrHTZE76rT1eEvfSnejYoPZ6YosaON%2Fl4kQF2rD3ui4qwbbLYFRVcRAOB4mGUGUoww5%2BKVuG%2BKcr6qhprk8aNyqeVkimFs3TfqaKG%2FeTxRhwzP0vg3sgdmPRHzypkPQZ5ZYD6s2Gpr5%2BKz6LSFsqUoLuoRZiRD%2BFRho3EcGr2ugRInce12gpikubq4x5qVTQJfuD5kXsdiMv&X-Amz-Signature=ed5741e4d3535376b96109bdbb713be7888ccc5cf605b4004c08b9bae14945b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

