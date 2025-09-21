---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHQWZTZO%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCy9YvNXzdLcrWf%2BENuC3IOUI1ilB5EvqFoNdokR%2Bw1GgIgE6OC%2F7CLMr4tNsQQ2nHGYeo%2BIaWkRS6h4JyYbJXHcHgq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDHavSz24sCh9zPVKyCrcA9kGV4Ud0qgglwzhPKUiuv4v%2FN%2FkFR442I5UCxphZoGCMGgo7BSsc53xU0JDsSUS7u1odJZ3WrzaHXx36J4D4upPdJrn7jgqO8sEIeUFhPxODJ04IklvkVCocguTy3EyOS3J9%2FhpOJ%2FmKgxTvBr%2F%2BLUD3wKj173KUw7n0D7tGdMi3XJVDOLYo1qhFxj0zREWYupOhEQE1SH1O9bf8plheGw6seArmtGqzwCLK0tgtv6N3RbEaYywh%2BxrCBgh%2FC3Y0G%2F651awJGO3HqB1OklCltK6DnhrORHWLlMk5fRt4lt41KhO6Q2dVmMGfihcaAKtI9KlkreQ%2FdlH%2FHHMDnW0nuLP7KG7UcqGY1Kg%2B5XBAUowKzchqxjhcBw56q0qWVqCrkfoUrN1j6%2FheoQi8zay1oFIIcwSgLz%2FLPIJEK%2BgX9MkPft%2B9lbUMBXgOK4Tr5nzndwd%2BmtBE00O5IyLHb5ovfBF9HIKZiKRNL59LX2u8ynKs%2BwmB6yEbKphHdBTpCjzhBZGWJUMPMOVdNA26WsGPuv024LxGXrt1s3Y9ARFma0WYelES9yqdjGA7M4YCCn6PW3c%2BJMGA1LYPZMPiuMG00AakDvVlyVE6ZEaLcVc0qvA%2F%2BjhZhNvWWqvm4tbMPTfwcYGOqUBcELv%2BuQWThIzzHeoi7kJp1lY7d1%2BpC6nZAvBgj0fCJRbWMlPzsEQj8SkmfWGsXCWTjK3b1aJcGgATEF8UK4No%2BJMdgGXw1log%2BZNuhnlYmmgMYZuJD0kwUl0vPLH5C5JBRVrB4Q8C5U%2FE0dniiKwXQciaj8eFY5SR2%2BZX3JtBkt%2BgRwwiVldVCoEQKp50BjPbNKqVn3W5YvF0dzb2ii02oYdeIDl&X-Amz-Signature=fb88ad73114f7f27dfdc08ff0a84bd4d465787e03c77505e38b508e226fae82f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

