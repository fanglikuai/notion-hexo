---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AIE2A6A%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDR%2Bi5MLWzKhPeD4gg37v50W%2B7fKFencrYqX3fgm9QkAiEAgfjGODgHgpQVWeyDpRU4BkA1kFLZ%2BSrK4phblf%2BiwRkq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDA5FuHDj%2BnGDKbSjuSrcA0Rqy0MwJh2zsNYQjjoDbYB1SBBOJ7NxTlY%2FF%2FV%2BQDfRIHR5I%2FjUKxEI8T1jo%2BbtmDBNz8xB2wkS6U8E052Y4QPorgu%2F6ZwZhs%2Bje2dPisuP7TmWcyje%2F2u4u99Cd6DXecaA1MuoUfw55R3aw43yp7sjkffUE4psITetfwoeYguFO0Y68048dvokZ4IJ9KcMbsBh8JTjyPRhhaQMrGJFOl%2BnTTvnnxaJR4Drx%2BCkPe6er5gclM6UrirBavPeY4OB3kS0lEGOOMRyQJZ8hy6MqKbG%2FZpNn1H9hwUCDCVR%2FJjMMsUvsjeCqBD8rvgbxWNGay%2Fu3giJq%2BB0jYJF8AwNUeJTDKYWfX4P3ZRewHsSdbzDEiTDe0Q%2BJYQg7GKa10rDt2JM6N4zorSZvEVaTzmzffeIPvc6NebBrud5RqiocDCOEooZD5hGJl6QFq9sdUCP4e%2FxwjAeO3FcoZiDoGa82KpJzHpRvHnZTsLDUkzB5aQEqI9UDg4dyeSt%2BdFJiBX4PLXnUFVfu%2BkN%2BNbFT%2FxVt1vhPSaJduTQWCg1sIOg0hPEm7Q6Ool28IqDYHlS9Ow7zLhkGA1cEGPhmD8m1Lj2TDntP3L5lFzNHyxyAtwkws8GmwiYOCZAEhi%2BeorFMKON%2FcYGOqUBqQ9Yol7lCF2HHW1p9VGD4SAtbrey6OG5%2ByQntaaLd86fcjpubNmykyXFLsyjlLPI3dZmdTDyT%2FStsuMGVJhzLPHfBFb6uNVNmrjbjiyLGP1QEH%2Fg2zFZnvcPZ%2Fib2ohEqmeflIQ3hhv6dcX8COJfgQ%2FKup19T6igXv8onrVdGBWBPZMsTIcQ0ZwMQpao6mxoqSaJRHQQPMNaMm40JI1%2Fn56A2tcH&X-Amz-Signature=900b6aa9825aaf4a8a904c5fcfb5e3c5ab94b0825445083bec8caa7d12236711&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

