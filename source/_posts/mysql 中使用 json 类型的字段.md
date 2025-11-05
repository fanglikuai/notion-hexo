---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDD2LZBO%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCL2NEao7jLZigIq78ypr%2BHdMC3nwcsVASunjoEflaqQQIhAN1pRAd8jlCdm%2BVN2%2FpqLyAjSrA25gsR%2FeHHcP4p9hhsKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9wnnteIA4bh6S3bcq3ANwnYA7dDjX2UKNTte2s0Q6l%2FLPgym4az9i2AdRipnNWs5M9607TW5cQq1YuWA%2BJ6ZxifM%2F47MfIT0F4yMLcM2WbEYbhUaSGtxsJXBU3GK35bPhRFDuJwVY8iy5466tHxOUjkFbYOX%2BQwp5bFvEJyguxYTMqsXtGds8O1AJMOhzVIAPSi52cngt6oNnJGrF8FG2o5%2BjqI47GNvAK0hQG4MdHZ3Mp2XWDoHDvmYTz0huY4JnEsR%2BesrrEFYuBxBvLg%2BV4qtMK2VzrncP8NFU8HdMvuafP47kxzh%2Fi2L9tPIjTaH%2B7ohbs54VOJ14HRdmePbu%2B6enIp%2F2UjPTc1ZnJuk7NydOM6VZbZlg29WqGFPpL%2FMVNNEDXYmzvohmZrp7GxK0PB66oUCIzeaCfM8Y5WS766qAy7%2Fm8aUqBygjV2F29s2Ey7M9GXVDXpiiPSg1IJU5Iffw5bboVDqLIGIy%2FSiLOlUS%2FFq3we2Hx783SUY1QYhxlVyd4T1Whuq3qJIKV01gj8%2F7OIbqgHuxppLq8kKrehXPPM1OKYGu4bP2iGd1nB%2Bwp2ehhjbnXTatzTKT3e8TwvLe%2Bc4of4nEJTSHkUprOH7LMX0uZl7gv6hMvB25Xegz9HyAmhV4ny%2Bj9TDb46vIBjqkAe5UyzLwSMB15tmg1Uodb27Q52ueNshpTgUTTqZiLAG0iufCSFURwzdJEkKRArg6F3P0FfcU%2FGmsSFC6O9YOsrSFqnl0a6C5MwFT5eXL8uFYiyPrlUE6bZdaDCfbSLXXaL1RpRuP65w6t9Xq%2FAh0GVNVrFpuDhxVmiODqssUXbf%2BmPuv12zLW3IvnNxinedmZ%2FFhFRmtmKCOItwvaPBvf%2BVqBTRt&X-Amz-Signature=24d00bace8dec3c7fd050671ef2ff640fe01877bda7b1867193b44db0fb42ea2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

