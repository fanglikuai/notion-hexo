---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HAGBH7H%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpYciofUUsq3cklprJ68PHmhzn5Q%2FRIs3l%2FZ8drJloWAiBZfgabvuOIDx0OreVWlkzulaHQQeMij76TzYlES8Jh2Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMVzamFRnL0A334MNbKtwDVWs8NAGYRG3FUKMjEja8dPT6ArJqO9VsPh1re1CfzKA8SRuwhuJKbY4cxDMhSZkfikPGs%2FGtRPQH%2Ff9H9MxvRjjVNt9H57dFfSMWDlzFxQOi%2F2m43OByNoGigvxfX%2BlLRC32znRpgTRvOs7sJzPEydx8M%2Fijn%2BWUlHb8LiFZZF34Uls4Xa5KcBAfVXHjp3rQjRqqYvmXxbI8yawfGUrY5wwuPQzMKnzToRtQdqlImNt02s71yONWcrb69CDrymiYbazhRQIAIok4GBgZmJyRyvos3TJ7JN%2FXQvuDlPw7fkvz2GFIQbBd%2BX8mO%2Bs1A3sBZbvdLnyemxlADdb9gmwM7J6hwpEDWiyRRr7l6mlhQae5aH0tkDR98OD3FPRt4By0R2eT6%2BRw%2BzYpw1D9LNtzMlxK6Fn9evVRpl1uUjxCvxPpuIDgf8Zh5wlcqrDYFzMNIL7xa%2BqcyAExhlqTzwvr38rFgCdSd%2BqPtBML0h0JVMc8mxUz%2Bl3INDzMr%2BCw1u%2FPcK4FGoD7gQDJPdBiV4RKOvwZ1H5NAt3aNzV5re12Y6Pre2gEUNB9he%2FJJXfNrpTbmmLiD%2FnuhomwoWwRm2FOcrQIR82UeW%2FvoniL7HGkJW9UxArWH0iyORU73FMwhIKIxwY6pgFbfO2oFBvLXD%2Fv%2BWha5NPPZAj5UFzZEPSSBcPmKF37B4%2FxEyXn%2Bdb%2F9500YGlPoDIf%2BnfAlqNItQ8R5%2B%2FBpVqiL5u9R17hXHXGTkbb3aWwjcE%2Bwk%2B7P9P%2FKBugyxNFeCWISpnT6WnNLtZ7Fsuk6wNuS1Z9qZK1szCo141hJLzOrZCRa1TC9Mr6%2Be21ZuFvHRdmvHZbrZUiAk3bwRIfLo4w%2BHnFNfyx&X-Amz-Signature=e7031c97aa0f05ff3c48ff41aa60871625c4e02db64bae56ae16eef9ccadfae8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

