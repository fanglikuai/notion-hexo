---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SY272JZY%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB05g27uq7xGZsn%2FEinmWF87%2Fhnz4pf39u4Ti9dH1bTLAiEAyEI25pU%2FHGWoeHbHUuu1eJL42s9838lj%2BmKGGwznjvYq%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDPrvlzXEqV1TxUCr2CrcAwZigrCMIdrVMu2QCuMSpCtJlyxoFAe99cIWuxGRNqJba9Oaw%2BcaxwkuZPhoYj7GURoT2njwEpMJbxWPE8TLJYNzwEtUjUhqIc1aSixLaTRzlFLdtgko%2BhLQMuypCYxvu1o2FqQz850j%2BSwAEu56BOfZaKsGVqAC3X6M%2FqNbuO2JwXdO%2BnisDNbjgZT8BumotGn54Nko3ftk8cOdYMKndPrECdhBMzR%2BtrznpRybyJ1hTBFJQ0c06vba%2BTHIkJ%2BeCvXYz0qF7jc0uZt43riCRTy0il464iM8W8XzT9Km0SZYXy9JPtNR31l77165Bi7tGqQwAnDSO6foNUwIMaPbACsstn28FdPBUcXW3N7vqXu8PXaouT%2B3wNUV44%2BIfc%2BSKZG1PCfl8EccrG7klOm5%2BaOzszTWztHwyt7eOwKcmz1uqFYHvMQmrpFi%2F57VS06Tz05nr%2FZZuZLALGyY1qhS%2B9Kem%2FIsZpr5h9h7tcJpwMTi%2FUmrFGW%2BOMdON3QH%2FI23PKbKXztRx3VjekAMcTooac%2BORqIT%2F65iU2JezqhSUKTZlcq5DjryR6Jz60%2FzX7zSv61EvSYd9bOHAI46gQ6JDDubeJ9mj3j%2FLW6JKuHT5UXOSjfMQjCxkww0w5JhMMWA18gGOqUBcAVTSVgIttbKPxEct3PrvC9NEHs1qYTemWYYcX26JYqOO585FUnmbqrN6lWE9XrCZSKfU4hEtnH0oUluMgFiZcZV8OwE4pFah5Ff0y7wcvD1jYHTifZ%2BbC94ZakL6FSVB%2FKElPrXVkJPQNgjiQPJZvWZc9wfJT5UoKubM8YbSv7tvEgs6hUvLrriHfRAH%2BesBI7Lc380nKJ%2F9GDvxaZbuLdKpXg8&X-Amz-Signature=247fb4c78d1ca7210a54b2b00717d505e108998be0b8e3e1e8733efaad89e976&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

