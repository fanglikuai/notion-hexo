---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WU7NXWA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbB1mImacR%2BYp0gkpViz0FZQBKy9K1G3oT9xSU9FDeEQIhAK7dbRbwvl6u1BLDmiJitIhK5PtOFAqAGl3JRNwzU5BRKv8DCE8QABoMNjM3NDIzMTgzODA1IgzRA0ynzAV%2Bwbf%2BX8cq3APlfB6ERQefA3gx5CUHXS10H3g2O913XESGhOfwAc%2F7%2F9YFaEyQ78k8fvtf6bdSM%2FG1A5XhbwBYN46dmd%2FTHc3ZNJ4Ahat%2BGNbU5doBmXoZ%2BjPJmj0OX6Ol59wI2DiOzJ3Z%2BB4Ljn5PB5ng5EZaseI%2FP8WWqdM%2BUKFOjiUzRGvXkcmfHPiHCgrEuW%2FafHhBpFoEh%2BOFLyaaaUuYIAwjOM6F5xeICV0B%2Fw%2BT4m4q%2FATQK0OgJF53fw9JOsdP28efqppqaw5EAJ25Dgvhg%2B%2BqGkJzi4%2BJJfCMcBaFpZzCHraIG3Kkr%2BS7fFGPqdWP1ZjT2KtjKhr3%2Bbcajw8WyG2vN3rcs3CiKoPL3XBEqlUFmJydU94lIlSO4mXJtKxfoqTKmEq6fo1nTst49TFIA%2FXG4MuugTZpk%2BS76UQdshRA%2B1COgiB37pQERJmdQgK%2BrZKxFM4q56WkdrqkAwB9tMpXxZ5%2BzJe3eaWi7nFBxqeOAqbFa%2FWZLJRSEx%2BLbyeu%2FYd7u3k57dF2Syg18vygHujrBhbvXu7vNQgx2hYEDIC2rAOcmFVsH5FH%2BTrpblm06ijzb3xyIaNDN6kZlOaS2kXKWVBFq5fWWnioMf74BIm6rxmIS7efA10WRDmXScSE7TCbyOrHBjqkAea8Mdu92zGJtIk1TNdEw0ykgcQvI8JC%2FtUs3TNOayAGzXatKlelxlyj5nSVOpGzdsBIaiLAcI%2Be7v3gwOaFuGGHjiAvpOtqFI3PijtP6w7Q98GDSmN7G2zYUeOZNUaW7IJILs4Xe4Sjj0tUMYwAklvTieM8tUhXngTvUJ4Fq96d4r1wvjEr7wTpHQee9PMBIKN2E0t%2FJGFXBVNlDz56G370DyNe&X-Amz-Signature=a27edfe9104e487b2af86f5e3fdacf9fb34785cb9664d5919c7d241e3e30c186&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

