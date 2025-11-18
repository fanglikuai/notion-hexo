---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2SJQ56K%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmXuT1bSKbQQRV01cHKHNMynluxothAb%2BwhNDaknGPUAiEAtpLdvuMh%2F7%2BLJ8qBrHoeOnX9Q%2BKYxetoSwtHVJct92MqiAQIuv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKfpeLcuGC1FsWgZzyrcA9qNcl9IcfVVHD%2BUXi2CoSfJpxAZssW%2FEHYNhpqHkQiALt99fdpov9VOPb19n5p%2FujIEiWVraTyDda8o0BVrfsLQ%2Fr3aaXeX1W8fGkN33I%2FUzAd%2B9qgBZtZ1zxm1xHOdyxm6I%2Fxe5P9dGgleXYMNKMIQyMA10%2F%2BoFWHC2xyIN1XfvEcacNHfYDkZB5Tzcv7gFdjrJUWbbVc%2FAUIPGKMTyaJtCxbzOdJXOCavN%2BbR9KbJHvThwypdkZ%2F5D3Og%2BV%2BGmO3YHGi6dC0ra3LTmUcBQYRr6UO%2BREw0K9RIHN7BdXkmwD8YZyKCx6qyYdoiTOpIfGSEnDOlo4Nm5g7zMs%2B2UihAfa6IvqELG8py%2BOFmaJX0yD9icCkXIIQLUzDenElz9fjgCq%2FW%2BLqY6h25yxWQyqgWGJPR8t7EIImgPgaMJdTd3xQLyq6aI4WwxhoDelUGORIOjDBBbFOHtDYmu1DgcP6Sdji%2FBT79fwNyHp02YqS1WH9%2BgAP6Kxt6yo6MItJ798kYEETpa890peoK6zMr1tH%2BZ4xGUTa48DdlRf0RJuVKqFxEexvEvjskfLKgTtv8zayI8UF14iPMxxurxzrBg1sZUXWFHyxfThhGoMSsmaWRXn4GHJqsD0MNCgxUMLyA78gGOqUB8SkpVITrSGW%2BQG3WyEmJusUJ2LcBwjowMJLjod5RscLIbyim%2BL%2BgVrIv7GM4H%2F5WHkk7M2sj2ahUp9xtPgqozvhuBkyp4D7%2FTDrfR2YOydTgKxJ8oBmSeBsoTXi25Wv2yPHzF4XvzvByVBfFnVijJ6d7vwTzGv0RySTXKav9KZHtvTRTymInqqi%2B7yFVwHPkUu8lrr%2BolCpj5tCM9vDyEZGE0Uwy&X-Amz-Signature=eee6dbfefbda5e13745c7536476f46a3b1e171551a6bbfb002a52415378c4ff9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

