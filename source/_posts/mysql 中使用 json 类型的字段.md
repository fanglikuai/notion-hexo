---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP3ATWCO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCTsCM7BEjnxuWzqa1AqcnBvmZtqMG6jKZGQpq7V6u5bwIgIoFGPFEtHoi6iUokaogTJt%2Fdy2QOT7Oeit6euxjYyZIqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0UulBf62i6zG44SircA9r4upvhJhgbLXsp7jB3madGzVNTT9EdNf9U1jjJTqOaLazFawSsGANZnJtJrgPvL3DQFJJlKvvD2hlNmSCGe3Ohz3lJ49k1PVmyrBGzvv6UjGzvu5uWICeHEuZmW%2FNcxrEvE6HFPsABPR%2BOfsZ3WyHQqaMxj4RAaCUZcxVIyAP%2FE%2BdIysDAmxPuWdG%2Ftc2S%2FfJrruD2VhWrQVld6b3%2B0DHhnzrPKdTI3O8Bz%2FdPKipcxioSWEBb6JpeSKwGwMRuC%2FFLWZzpXu1QexQHRqEDotubTNs2H5KKdGb2jpDsmVEpobhAFwXdKaGmr1HaLw0cdwooVEY%2FVQKrzjipzuye8ahZDRPlCRElwQxGSyqZK0PTo4Q31UfRazsV9CuJ12c%2FMz6OMaRKPvzXO0IU5GSgerHyJidRCjYBzBG5WWn5lrlaZbywXeP%2FEKtoltLalC6qkjV0v7OIaYWkoM4VmxGWuHfYczdLyDruk9N4PPC0TDP2Mwt5cBpEjxgb3xxmqaQNyUsZGGY5i%2B48ULg%2FAuz7eBo6Q56ellu%2B3XricyPhLKGxvllmTRtn%2FO%2BZX476jFXIN%2Fno9%2FCQpYJVTjkIngYmGl1VHHCD6warzTkCK1Tvpo5igtG1%2BAki1ZhGLkHXMP6OmccGOqUBYxi6FCD7jl57FUJS9P1RYX6k9ITEgtkEXcxT0525ZCxJy2PLQx7QcOptBkXnzFdtG775h%2FWAo3mD86ttHgXRxJ2V0U9Tp5ZJNylM5KB6y3Lb%2B4TXC2tHiltDq6xk8C1uy4XO3jWbKpTwx3LRHjcADZOyLQ3yTif8SrMCLPNL6Unm485U8%2F0OShvkg88asrCReW%2BFVBpAmE4JInCArPC6EBy8i3j%2B&X-Amz-Signature=2a7278c2bc91d9106d9f7ad20a7e3bc44e2bdc1eb125d225b4e3d97cc1eceaf8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

