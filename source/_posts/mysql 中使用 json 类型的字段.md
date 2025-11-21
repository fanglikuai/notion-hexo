---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNPQCPLK%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T140117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJHMEUCIFfBeOt4gU8wL3TEY7MtyF3Z1FMwrwT4zqykjXEpd7D2AiEAjI0YwN0uMb9awZs3ZhTVjBGGtVTTvPisecyB3albpFsq%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDMPpE9Lo6Y5NywoUXircA6kXaTCFd9E0EX47Uz7dn6va%2F7A%2B5PzhMxrkfN34Q91ZPAgHeB9cMJA6XaqWm7u5qO48oFQttiIAnGsA3FI%2BpSRvq7Pl%2F3aBhCBVM3SK87SjE9DuOh5HLKB94eKvEnJYh4BYuGHUjMxYG1aeebbgNXqKYHKQLmAj1YFqua%2BSI3dxRPoRrQw1%2Bn6zigd801I%2B4B6YLDN2xk4ty19xp9tXWya5K2%2Fj7LhTRbHZAWqpgayBSF4IWGrm39tPLk5rH%2B%2Bev5Y%2FTGuK6syaGNKEUqY9%2F33Rg1olNL9yfu%2FlfbD1yUp6NmTycLbkJ%2FwGSSrGuGyyOrdotXPhWn%2BUg3FoRILn66AM3Zq9XgRs%2BOS3pCH6VkWhbqHGCodcrWAdWPChAAX%2BMuzvSmCY5UyBAMikWYrRYwQBcNYdnERJFkiIIcB3rUZROqGUa2ZJGL23KsPb5G7WL1K%2BhzIcGTC6HNqiwKGaoEMnavxW9%2FHcbLKcK40VnqXE7%2BNGZTZYUE2toGPeXEFCbj7u2hSkmOefV0SzRarYjCaFZiLHuexvYaK0606jDML3SbGOHQhzY1xFQM%2FqyAz3Z3JsRfpiPBdzVrP36jtIDRjXkCov3q5dg2pSlkCq66mNDQvwb%2FsE0Tzgb078MM3QgckGOqUBWG2KtFI2LepEgnVl2hIv3oZgJwWBmzrOO3%2Fl1axqk9UApwVamrFCnWXnXktrBCPb04b3IhAUl8kkyYvR5rE%2BC1PambVqXxcn%2BNF5VyMXkasYwgSyV0Gf%2B3FnqQYToVLVDw9Q2fJJ9olJ9Nw1e8hYIBCL9AboYSv7KnLca%2FE3SRYJLbFoiRN05FY%2FeZgXe6Z%2BaIQWiU6LtGIgFINXWiA6bbpCU3oG&X-Amz-Signature=3963b0fb764b197f229dffc0aceb4955c4b3005dd743fb86a7e3dfb483a67227&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

