---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672DOM3S2%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T030058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDe9uZR62RX%2BYz3pS3kzbTwCH9gP07V7MZeDWrxyEM1DgIgYMNvecYNC%2F2J64BawkAuHfhi4k1zzznks458dW7Ehdwq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDAzaFTkDV35M4jCLMircA9Wkdk%2BWAeIxpKNQHyeo7HG945mHlGI2NAbh9z8Gy5uGldiFTbB7Db71mXfC2xCZFQzf3%2BfcJpEBH6ReEJR4ed15DQP%2BYVlbLwzr5qfpsFiwCchLf5DBIlQQzuL8iiLntgM2Q6yvspR4ctSJGGvsEGwyClkXriCMKTDXSPLYLBchvbpWtEYrqHzhNknz5vyhjnaHrA%2FL3ZJw5TjYsBFmbE%2BwoRCeXNjAOUSFysjBLZ34At4I5ly%2B9wc4k6tfcx9y%2Fwo0DsHhkwpp%2FvLWSMOTJNJXfK1Cp%2B3Rk8bd7Yh%2Fc54wfjZjDk3GfoiALrpgsqsPm1jAuH321gC7WOju8COFiZCEWkSSsv6wNQR%2Fkju8XEYJRLWe4HlTMfi2Ic2X5TdZYwEXlDlteidR4qI8CHArP4WQ61K4nGBFxD4nmfvFeXKVbDu%2BMm9ss4DI%2BcApRin6BgyF1BWTq1TNWaXw321n0jagMyiNFroKT1ih7XBufciKVxXvqzlRAcEAasclwGZvCUypbvfKMUCvCda5Nnrh2Xcp57i78oOd1cYzGPEyHaOUbuOITgaBfnnbMwuvJ2ZB0X%2BQY%2FkdirankmOAPNDZZc5uR%2FjUdyC%2Bo59WW%2FBZ3nuYLcob5o6r9NLx5TpOMPyd5scGOqUBARX4KiLLFuEiiHOjX9D0KGSAVt4no9LzmjZE%2BPUjI5RzFjKUT%2F6670ql1ucCQh58P3z4%2FiWxX%2BdbYV8xEBOHwjzVRY8Gi6hl517EiTHhCmXHTIZOBAKqu3wGXnA%2FlgVomh3%2BCHTcBfNU5dRzNXjpeoJFUJtgdLs2VFn5h6qhfeBxE1QYlKWuNUVleWDpkiELBEVTkocdFCQvNRPhDBWPR6hAGT0D&X-Amz-Signature=89298cd8301adb60e89b0028797bd784c49fdf972e5d794e841f923b04d1b770&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

