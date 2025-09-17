---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46645YQRRM6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T040038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIG3k6AROPt6wJyjn%2BeEkBo3qhiHZRgrhu%2FGRKc6meXKqAiEAhNar0DiODfMhQ3FTCZSDkDG36ktfE3XJlGFNwEQQzBcqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJspIDfRBV5Ls%2BlM8SrcA9PyL5L6FsbGw1GU6ezLbJm8SIfwqQppAuDGRTpLxRWj%2FRYnaY16vR0G0knTtxoNRLfhKvOh1N42HTtjBTfY0zAVH8g%2FAZJruCg993YG9GV6skCqPegyHLcDDu%2F%2FpErE10WCz2voeHVN%2Fq1XnlTHeJwRLeM44kbECf2WuJ5p3BsBvLmFgkQQpEYjD7Vor83GmA8r0Hx6IX7GQDzawvg9Qd95Av8lA2tanQWccEggMFOsApcP1KruG31VLcFm0KbdKjNQ%2BXb%2FjdTMjYZsKMD3rMl3IRxgZ%2F6wPrAvwMd1M0RMUFN%2BDGhhn63smz9cnqjAKlp3fGRaDLs2TbV6kan3W2hbuVm92E7Ci9HxZBuYrqivvO0xlBVvZnbxEiMOd2Kv4pWabCmfeMWXKM6i%2FaRaR%2FTVeN2ZK3KwhQBiiU4DwzU4mjGVdRkNC6%2FVmrj57IkGr9p9RWQDrh3%2BWIP6WMS6DBzfD617xgQvGnM8W0OVVSD47rHuYRKX%2F8KeSJnizYs6RgzucB0SM0XVOvhq3CFBTluQYP50KOonvS0xwAfegL8XlcrL35RkYpll9BUtw006AwKXvsaQet6vpD6EOKdkfH55BYoyPhZBeVUeYnxBY1Qa5toRfceG5V37gGuvMKTSqMYGOqUBi8CW%2F9mP1omrX1vXnnd1WKq1JMik6OhCY22fUGyydP8I%2BTMJod3BwldW6A4xJec4P2Dh%2Bcy0effE1fOSnXX%2B%2FdB4ibcnHgHjmak9CipKqOu1kFw18gARXyeh97dDmPSrckEKGf20ctCf6gbKClJeSlhk4yyw6PBYVXNzeH4fo2dqMbxvjpERdHBcBLf5Eb2reGZtIIHcdKEaOqDspsVA58VToAT6&X-Amz-Signature=b43a493c7d7da4c331152c09c27fc99cd23cba25906b2400c2f906544c78829f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

