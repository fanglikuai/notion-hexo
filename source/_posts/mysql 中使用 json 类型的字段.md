---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM56SZN6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCICCRQywGn33J3ZCF5SyvCgQPhD4BJoH3Nvm1tZ%2BVMe7tAiEA7TTyGUhWU%2FyZQzHNtJkqxoZaWKD%2Bm5O9Y8dmQw499wMqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCTHg8yRexoPLWHUxSrcAxrXKOpIMa5OMhgd2LcLoyNw%2FUJhUW4GAieI%2BBeg6L8FHf61T85OGJnMAlnivzXbdWVTTPZ9iTaLI9ZjDGgg9SJxfPDBlpKD29P%2BcLvgcicJfEwJbopAgEFFSH4bsn635qGWlPcWJXNXQQ8I7EiatEAZxEUWB4%2FHFoCNbAYrWZJpVm3LLFjPxHWw2RvcUwmktIe3CBZlFTbrMnEXGRgAuDDIVZWgstoq%2BmNKPsHDtyTtaoMVTt0PS0x8cutTWn%2Bnld2kEaiS0fUesIaKI6Sb%2Fn1rfntbA0wXevHQODGJF56Ho8ugtR30OgSSPQ7fcnGHiULw%2FYaRz6qg7ae7cSXf4zwM58B%2FKwXh%2FWt5SDow0LpZ4zBTi9KcvrruNyU%2BaRev%2Fzc%2FTNq20fOqLN7xxOosN4sknDFYqxAbbNPUY%2FHqLCLdiCA2i1a50Kit52kiIxWMthwlAf4ezLNLpwqqy3q0i9zTYF%2FujnwaxpWVfworg3oxwJaJSSxZQd7AnwI%2FW6A4vWQhv2WJhl6g8n0QyDucNSzcJ5gQuomM37n%2FiCga%2Fw1qWdqiBZVsYtu9J8ezygklIujUI1PBuD0chsvdFOgac8gNvs4pcLidbS4EMBE0iMPAqDJ13oL%2FUeqA2Y0fMP%2Fbp8YGOqUBZnwxdBNmckSE3dazoH2r6Kkx%2BGfpWX83PnFTxLuwXdCWkwjXbwblXaH%2B4ALtZCSJ6i0Nlpm%2BpM5BATsAfeJfJICVmlNmQeZJw5XWOBCBwvqQeAjYf2n1RXzEE9YimSXmLaMuzbut5CT2bvpBzOcmG6GGL3F5TmaljtfqkPhkKeifulycE2os7DcL8WBOymIUFppCHz9uhxtBEDn52kSJl1oY2WYP&X-Amz-Signature=906f5c59f49aa19247e2fb8e3361870deb35fc7cccbb341a1c6b67f30addab61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

