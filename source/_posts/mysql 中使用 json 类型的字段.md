---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBSVVRMC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGgD%2F5HPmUlfXcEn%2FbeSkg9Vvq%2F2LLjIK54HYhSBV83QIhAOnmZ1w7%2Bh84pWicvHxEeVyaUHQPIjBAKp2bdoGYpNMYKv8DCEQQABoMNjM3NDIzMTgzODA1IgwP%2FBZCmXtFF3AKmDMq3AMMhBxgZ2seESBKKZ%2BjXul6HfWdAuhBcFh5XTjS0wGUKCL1d0xQfDH0Jm2S567Pk2M9hJUJXv1wcMCrmkE5b2mNaUV3Rt6UduZNB6qi8Ha1w4j9hkR7BcEBf85XO3uhaLnl5pGWlT9AVEY9MEhTnahLoMTvrtaM5Gu%2FkmGoHxsCwMiDV3FRYBRdHVUSzEFedkr10PLeQ2DUHGXRNv15GjSNB27f60gNMAfEBFqGX7U%2BltGMSIW%2F8udB59Rg75bkVz3ZMpMa2quz6Cqu8HThvwmIcK9qgAK4HudSOmmwPhikMXdoAX8ZftyLG5KZcW8yPyCIXP9nM7H5%2BFyv1Y73mFvW2WnNgcWaJoUEnT6nkPwvt%2FXccu%2FwALdzN7ps6vY6jerdgLXfQXWfo0Qk31vwGR%2FrzyHS9GBxfeOiE34QlIfiM0ZdQ4VJUnlTiE78ZkO6wsVmT4Wv9sBrHXEnBeDErSfb6MeyvzvehaMqxpaM3Mov2rxLVUB4pEXKpC5tHYSL%2F2Z1ScK%2BbIp0Su2DGNdDgcVEl6mbCZAw555vKGIKSuHi1qjElpjScshylk58QNFG7rY2C0pXRcnSBTCN5wLngCebPYmP2HTO0%2BKFqED1yFOln3qF4IRVbQ5fWwBaDjD8pbPHBjqkAZ%2BA1FalGNhvK3467mOlWnU4CpHGhOzReI2vXnA8ve0oIkZkSGEXpPYHFZaHRVflOLhJaQ1Ys3kvIrotifkwN6GeUSpfpcFIMKxXwpOBIfvoDhkrVUiQ2WX94FbEHBi4F6rgjqKObM9ntMVZUs3HO9hpOZJPftynRRmwTUSnUBJjMq8xSugBFjifkx9q%2FKLW9xPsYspq1mcg%2FHx53%2B2XULpdGNtJ&X-Amz-Signature=115cc5a6313cf54d2d2c14e25304319ccbabdc060f44d1ca3a176fd760767e3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

