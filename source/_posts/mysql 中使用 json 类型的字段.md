---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2MBRSHO%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCb4OIVOVIceHBCqbxzfpy%2BslRaZ9g3JIeBr%2BHN3sOmwwIgUS1XODNu3%2BLMluO8bXJh6%2BvebF3zIGhB1CmkzrtKdpwq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDPD2A4tIPaTxfu%2BNcCrcA2f5%2BVHWJaaXVCehWiP%2BAlAQ%2FuCUfrXrltKSA%2BcXSiOdZ9t15eHjQTZn9E%2BZh%2FdMxqfSuL%2FFTk9DhQMnfNBXlwPcn0DEwev26b8IyJVuYQBH1LYiU2rKFMJrkBJ8BTi4Miz1Hm6ihu%2FdYAwHNww%2BqEaT9tNb39b1m5cEan79FnodD%2BTXZMlLU%2BmlHkQJ%2FA2FWZOD7HoE7BVAGCqO2bPq%2B5EK7CxGRd8ODj1WJnq5iOfKN61N8kJxm793PvY8f7mvuLm7L1isvUPB4jXqHZzRgxlfeZvu9eG%2F2lqPW3djp7XuOTbxMFLq3Pg1LN6XcmEssF8mgk3J%2FTJhCTcvdtX5EU2E9JCE9oXdxlbAw3KXLwAdfrqBvuPfseDE4c%2FeJeMX56jyVHdbqgtG1ZjhjchekJp9GYQRQ%2BzRIV0w0X7g4h4zk58aqNrtLXZJNxnyxJHD6gmchfAX%2BYh1W2la9l%2FKcT%2F6s9%2BPZLkn6naVC0EYRv8blHhwvTOpMINSo5FEFrLYC%2FxuzpsFxg23F53gV80abVaqc9jnB%2BRwpkuFVHO3591v1AhEhyA5XE5sPJdsTwxdrhitD%2Bk4JrJXbHGDC%2BB04Gt4qJctWWvH4R9GOe5l96L8SMX%2FFej%2F54zuW5GFMPT%2F5scGOqUBrlrrATuM2Ne%2BnNE1Uz38JfafUa36pwjSuoBoLabjFuXPQObJApJrOfLIJ8ThSaxbtEPvop4iMu47aqKnvSQnNQEGRgTN9QkW13wEGQKLbgeI4sGnJBAC9275%2FJYG2ZrqDdeZ9jB6OmcIQ65v0TpJ5xEIPMypA%2BXPJDVybn7tp65DH1u2ECCg%2FoXwxCDq%2BhaYmZDhCjK4eyvFLpMS47jnmZOP%2FFpZ&X-Amz-Signature=2f41ca87d3a3a24003a613cc2f32204189aef117243362b8bb7f9358c1030501&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

