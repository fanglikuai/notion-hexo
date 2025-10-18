---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM7O76AP%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDUx6AZKS9z2xAnX%2Fx0AoHLYUAXvkNWhcL%2BKl1SYpwfqQIgXW9cemByUR%2BgwdZM%2Fw71bxGvImW%2BlWi0iNJg6OLcGmAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJa5VLpKuFLvp7rWPCrcA0kY8aK6Fg3TXJpmt8GX1C0%2Fuu5kdRvm%2BUB5UZXARijQVsNyRVxsKokY01kVsJ9KSrRMUXTDaWaXN3JCGvc%2Bs%2BpyZfPpjPS63t1xo8uOxjj4nRVIuU6M55xUMo4Fh5VFLA8kJ2bWHX5fMyjXbV2oGfvS52pJEMf9klKANVsUAUO65TjnE6nuElOCTRznzqB3F4NAlyEPerEDOVbRqPZmxl%2BX7LqScrJNsLk3c9VJohaKnx9JSKW1XfYHU%2F804%2FZR8cHF98Dfr50kfCwKW3jv1nUimLVy%2F3UU5UYhggVH2qYg7uatIbUegMdgok9Riq%2B%2B6VpZ252aAhtYxdAiYOJuhEejXTEdMbcovnW%2F0IV5Odqm0345UqY0lU23iUfmDcOpTseH8Tnr%2BWCk17Ibab6k9t8HGuxC3HP9wpKkdci%2BTAdrVk1dSus0vt%2BHnvP5P7BobN58nHy3z0EgBE0dv8niJQ3Cg1DbVirtFhzeHftdp3IKtJ7upp5lphqvdgAeXD3B5x2dm1nzHxeN8SHX%2FITXu4%2FA9uAvHQ9XYxJLylKsqSQPhF5vQv3D534gOFInfDpmwN4hzT1yFx9L00FVE7VTh%2BJZIqnK5qJWXo%2FzB6deNjSp7%2FniSO1qYO3xbo9OMIfhy8cGOqUBpbWBABgey0OuQLVj3LixrC1LHbzF4oKpe8oAmV7Wbcn1Ct2n%2BmXAjNcPCOfnDNlZOPUjHf7UQnsfqui%2FC2Y%2BT3Y9SvLeja376n0KBwsXvKi0%2BykwcxI%2BSMBRuc43aevl8XQqxtvcqWSjQ2wZ9Mh8wPTeVLasqk3YWkYG%2FnMoHWAEgl6Ue1gSfw%2BKHiRpneowEPB%2B88ehUNdr09f4dtaDeEJDF7wA&X-Amz-Signature=a69d683f3cbe8d651200d164b969cd33555732ff48c6318dba7fb32dff08d3e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

