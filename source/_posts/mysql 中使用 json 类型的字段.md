---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS3LAL74%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAW6AkXeOtSQvvWfBeE189pJzfx%2FIa%2Bmy5oHmtztU1C2AiEA6OvOi1kLRFdT6BP3QmS%2BT4ih4CH2lka%2F4JhC8qNBtQoqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHesuP9ZdIz43nhpnyrcAyYVLMTtjzzx1HIKn%2BgmRl%2F8dInxpwgGllrKh%2FZIjuOZqr%2BHgOLFB4HayIQbuVs8AG8Ad3LLOo7dJ4Ip0GJNTz99HGH%2BGMnUvI7bvB8R7Lj0fr1xuijbU7p0iL8s8k6CNckgvpbfxIBhbUMV%2BMpRgRSlyxMDGRfChpH2j6Znl0YjTeTCAi%2FEh4264nJN6Iw9hAFmUBNRcCrKyrTK1m8PpGP7coMpCamD8clvqOT2ruwZ2V6mNjY5cVdngc%2Bpmta%2BPUKh3%2BZ%2FJf2x1Bb3qhN2N%2FQr%2BhfYnk9HZ1T0ex0WaHAs5Ofnx%2F2ri6unTiapDj8vQyPVQ0b5d3lPsJkXb043RawnX%2BhhCuclKDS8%2Bxgbj9Skh6C2POP57iTSjx6mSE59W248evSYID%2FrRZPsoIkLhtv7P28HrBd%2F9%2F3RkcPLafVt7wQ2ErOSzaTE0JCEuvIUl7B5viRcwUYQ4n31goIWx%2BR4mZeiwuVEiI98ceEC8eRU9AQIO8yEhhStGud%2FMt9WohYdFr%2FRByQjlysZlnY5xBCdyl511SyYVV75oDNTJpDS8pnvy9jJnLxjXtuC8VNEhB60UVV0UBa%2B31K2GmQ0cn0t9yIg5dYCn6R40FqlYtS4zf1A886OiGGmAid7MNPPj8gGOqUBqgAs36rkZfsss%2BUk7QoEmURCfipYsIS9sLHNO0p2IQg5hpFwz3raJyClfjhkHzOy1wYKms8ep6zz%2Bt1bepA9468ERtP554%2Bww5svI8gE6NBRgUso%2BLLEZnpPekCzdkSrxoj9CqWuHtGlZU3r7YUUaJdOeBGxzfYFVGmrxclOmHZf3D3FT64vdYTsccWRiezSk4D%2FJ8apScudRjywY7k%2FvSVlzRYG&X-Amz-Signature=a530c4020c670ba70431a74dd99d6410967ec21f407a1cfe65273345c661e011&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

