---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JRYWWOR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIHOhbPQ1dQ4JpyxJG7vIB1s6JBcRYIa%2BlP%2B4VRljj3bXAiEAv2gr1BKbOPo5%2BaizadYur9YX0gmsgmKd8yq8QRQiWCQqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBiiqd40hw3cVoVfPyrcAxo%2FhsFxKS1U3ZLU8nuFN7qX92Ya6Befuj4Fa0jjNWnBDWtSpS8YiqEYTy1DIF5vN9z531m39brFu85G6MNJlsiXfbKiDsI%2FzXJgf9HD%2FzlJgXy8Qetsa99dkXJopL7i3Yw6mnaM%2BIzXpSrt7SrmzYxUTUsjvk85WlOI2HW%2BUXVieW%2FwdNUBUo5wSKHgx2IrEopl33ieT6RjQN42rHiKeQZuttNUCoV7BBFk1vHm%2BAN8sksilh02u%2BuiAUkaHRtL%2BJmw%2BpcPvBrzAG1VwrAH2bgKlue56%2F35xP%2BMxPrdoRDoqArFKVhWc58oJTYIOWNF4QWfJYOlXW5UlZJKv7venBXm8BYkNy0ara0J%2FjRx4REMBjHio8FMyJGMDwV4WQPZoakaH6omyrKSw3ejU3NKypisaWhpvkycivwvhxzSmuEeV5uBAjmwAH%2B4Mb7YoCKhVEmGVFSyGjw8ruRxD1hZFDf9MWmMRIupHO1p8jV24QHommXRxMFGTrFdOvVIbkaQmXk8LVn6XZPJJLh6jKtWw1syiFPhSRMgDRK612OSd7I%2BI964vcdm6XZckC%2FsLDtOyQUtMzZ0GdtQby%2BlPhMTwtXvgT%2FrrNFKX9NDOxSVYOkiohEt1S%2F7wmjB88cbMIr2kccGOqUBAlIJNP6hz%2BatKrHaAf6kpjimIfnZIybFgU3hFpb5Zci%2Bol5d6FvRpRzXzr5nCuodflz4CEMGlD0qDA%2BX9VEVjCI68DoaIbaT0KL9nzylrKnL3vt%2FOCxNQaK3WMu39vjvzNGNZYWIP8OYeqRr8BlyNQefvfjQOBL34P9tmuyfBHnh%2FFg7a%2FnL5ZkAruM89dWq6N1JSvR%2F4vlWDa7fBMQZGU1Ibyfk&X-Amz-Signature=61727600f6814ecf5bb1c90b1545276884c714d70795a8d479b26dd63de029cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

