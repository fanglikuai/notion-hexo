---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KPFY3BU%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIFljmd2neUbbP1Cz%2BbQjmE5tXNmFQ0KPxgjFS6aywKDMAiEAiIXm42kcwdORicZDsmVljb4HzEkAx0%2Br8PGg6Fu16PUqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKTC35i6QBSDUVBGryrcA9N083XoKXi33iJFbqs%2BIhRBMXRxM928FkA71UOt0ogjL%2F2HLOMcmd8x64%2FzidOXIoVUGp%2F4HSvvBcAjbgg6hm4yjvEPlJZUY94Sw9nMvkfNkvXIIHYcbsypISJHMjxXmsN2HIRQPSt3EDWjkSP8ymvBId4zVVwIdvyCrmgrJT1jy0oOqzsLTRW2cipx6APdiFuJHcwkYH7tho5CqgvAw2h1lhGlNSI503wdhQHBi8EGITWQaYghbRWbAnSWNYcPf6OncRQeHR9xHy8gUmHJASMypSaezlTjV15hWu7A4UOXIP%2B6O5gX2O733OzZmkUkakyahvXKnyWtYhcf1pp0oXftvVqZC%2BvfKyrL1y7jaJuk4VE1uZUN%2Bo%2BigDW%2Bq8In77oMLZnL4z1Ju6TEPl%2FQdvM1JGSNUWnfQ6zIpx94%2F%2FxIeuiZiYNpuZ%2FPssaYnpGdkEBuuT4%2B9ptZnBol2wUiJj8SiaIYvKrfINCOQcEtn3t24e%2FV2J8ceuVirfTxQUxxNQezVmuXeaU2RsRpN1p4%2FdGuNPYrfh8C9yHLwmxMSY28szRdPrBWp3QD2XdfXufyYSr%2Bysqkn%2Bt%2B4iaJmrDKGNoo%2By%2FHKjphEMpdFHqSDH%2FWrJOC%2F4IpFC2%2Fv%2BzFMK73nMcGOqUBmhN7w3phjIxIHHOAIMopSESkTpqRIJ6dqzQ8EA9b2NJzbUPPD85IRJ%2B1ueEtsc%2Frck0ikxMiIoz9ptC%2F1qWU%2F0WpXZLWJFKqCMLDEgq%2FhaF25ieWBFuI6xsjs2fJW7XG1Gwb9xs01F3uuhi5gOxE8JpvvQklMUoknyVSBx8kiHPjwTiEhKHtK6nrhdpAfe7S1UsQDcUhI%2B6MLlU%2BfCLjU%2Fbd4h%2F%2F&X-Amz-Signature=bdaea266281defb1f263518cc7505b509c1d05f7523bb4a863adcee2861ccd14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

