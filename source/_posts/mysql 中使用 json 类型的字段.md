---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665STVDGUS%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQDl0TP2WRt87UTFmdQZUNPdSNMFiZjWP9XBLlYxIpcxvgIgOCEOWjNbbO0srxI2TzoW6ecLYbbil3gfWhtIFEE20a0qiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMtQxSz4ZyR2u%2F%2FmCrcA%2BaHjBFMuHo3AHsjVGPvf%2FlZxRF6JunZwwVxOXPIYhTcDZgoMTi1DWVscl5fTgStlvhHShJKbvpL%2B6U9Y4R9%2Fn4cKr6bmIeJw3SA4S1I2LL%2BvBQlj5GwJv0Nf%2BgPMeewHxxh8j%2FVtqp1BTR%2F2yn7zTv%2Ff2VQk9lV0FWBBQSzFjfK0T9jsSmLfJe1KoqrLxPsR45ajFd7zuJe6cXEgzCcSRUHyQ0KSAI3K8Fy5bzOMs8HcPBIYI7kaOsWu9zVOiq2avcoQsKiBd03%2FHuw5Yk8HRYwi2UCmqEUzhDHNY4rSaP7yxU7%2FVTTy2UD6nvSXTTsAkrtN8WfDVNQI6b4znMb%2Fa2D3RZJENkYWVyq7MyLp47tqu7BJAc4ONheJ793teTntqdz5AJA7k5djcGVdmobA5%2B1y8XEloxyyrNQ%2FoR0Zkl9wtI4t6zHNkRIsQ7aV4TAlrA5xN99fmceKW2GXe4JhcpvsQwQMAfm9b4JnWHfIfzdyleQaOQ%2FC6Y2VHMVug%2FHlqAXzsugxE5ONgvyq6CeMJzdxHgMz8TrnvRzGWW3izHvmFCmBtOtXZD5bribHpuN06Qhk4TQlccq6gFkbycJ1TNW2P%2B4rvQf2dgtiXuf%2F3UCHPkKl73ADewdJzOMMMW%2F7sYGOqUBpfnSTt8xPRzf2U7catW6xacb%2BkLFSmvzCCO5wPtmJ7mJBEndq0TgRg8qtd757Vnk8L37ujnqSHSf667HPkRU0odHRgTc7H36%2Bt8bIz0oRujdkYqj79bdLuWEyH3aACHOX5jgCbr6KQB%2BbAsJg4MpFOfCbFiIkTWFC347D%2BKY8AP9XgJZqgmuIAXyTrggNjAhM0HzBtamnA5SH%2F8AX8EkIR5Y8FQq&X-Amz-Signature=ed1a3c02becfa47368d15fd50ac0332111d9bab875229b9ffa35ae1dd52aa7d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

