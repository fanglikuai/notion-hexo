---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4RBBRUZ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T190046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQD6hOwCggeHD5w7NuNthLpNoxs%2BfS0sKhVG3051etKdmAIgDwsc%2BDVUUWU6WkvUdfy9k5%2B8%2FAJheag0FSnWGOtP7s8q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGzHC7JZW%2BPpF2GVwyrcA609%2FIHP2h%2B3heGhDlyQVIEh4lCCGn3AmVMmWDDmZfoB%2B8VEpEiyJay8Oa0eM8i13gI4YmthVPmJEf0lOeb19uGqimFO%2BVtMtR4XeHWi8qcDhV%2FpJbYOYYLfE5F4rYuGlw6scevCmD27QqYuHRIpXBiQu%2BxrpCphTrfR4KGACa6aajp9ezOCwpksVaiIqOvpKkBDWcEKf%2B51pVdwOy5EF1kfZ%2Fa6pC3nclqRJ73iXLcCH9fLTo%2Broq5UTTB6anxvMgxDX4lY9wB4ZdkX1HQPep92ydnXJU7xOqo5VWDw5goIpBusbiZutMrVGndAIkhvrKr616bjxWIERQ8JS0hsVsHvmVb1iMt8%2FYYZqfEdJM3BQ4U3FJMZ7d7%2FLn5opSfz7n0iaqi570EY2GCnkYR1xsCF25OHM9vYWN9juftxILqCbORTEN%2FKccxrmHBi0aMnHyLhDR8yq%2FHxg7eImDueBFAIdxVvzB%2FhORQeiavhg8iA25TTM7VuZ5azR0Vdn4MqQiG6fDRGzhZsP5a0PhB%2FN0RyAt9r2%2B56%2F1HiTZ2HJJeMteU97BS3QPwZd50sE6hA4xD5oM1tEFeRUtKCcYccF66UVEZwnzfBLNv9pVAoihDEpQV7ckrps0XOrNxWMObLyMgGOqUBS9vv9B1ua6penmmEidaw3BqBVwc4VrhqyldZtRkwsURCuWYuOG0RZfB%2FBKXl4lsuIB%2Few4cJuQURkPL67qYSVQW6g47%2B5giawNEUjkhwJWM%2FVVpX6g5SrvBvUs69P%2FY1%2Ftv2259huyzKIZrB4h3XyUaNbE7pYHXmHse6tCqX3ZTSTnd5vWWOJkZo3LxpHIimQdokLOTOYAAI6ArazbQTmsHaptoY&X-Amz-Signature=21913cae3737a0360fc614a8edc74bfb0840b73da18fe27bd9f38116485312d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

