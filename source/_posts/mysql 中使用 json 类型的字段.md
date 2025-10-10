---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RXROGNR%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJHMEUCIBUYjZ8r%2B2Vjo%2FHMPY%2BQTYk7S8aOF3VkPiVdzrMpVdkLAiEA19F2w%2Ftm6SSn6f%2FnOXnvImGZT9GyEPB2OQptLfiZhsUqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLJ%2F7AnpgA0TO3PJvircAxMkV6%2B6%2FHkqkG%2BzAUQZ5bpzpoIqBNAgQp1oKwiHubXRwFUzISrcytosi%2BwMFe96I7dNDyw0RSPUchsmApOEMG88LdBSJ2WZxyhR6NdKgPb9HhzIasCjfHfvbeNeSMLBxpWSiZL%2Fjn9Ze1RUcnkXJX3g0bAn%2BmBqXCQfHFVuwdnEeukF7QtzJXjmkAh4iuquz%2BAPzS0JRUKzazp%2FhNxMYwsU1W5vLnYHY1Zre3tVsezQzNRTXWFckC0tyNXHVXWGQoeFchNIgGrSc5IilquRtG94fa%2BZZGkQA9%2Ffg8FTY11B0g1cC0kKb8foreLVoOtb9jTTA0RsSIeI41tViiuiuRvJsywa%2Bb3RFzeOWDNporiMxrlsn7nEpvKp01kqzG6tJNQHCLbMB4wzPc4Q%2F4piJGbz1nFwym8mR%2BOyaucsXagy%2BR39dtJa6FyeNY7i0F3IVlVGFvIPrJE%2BDu2H0XS6L9T31Bv9yB%2BN%2Fx9LFLXSKjAaTn7gE6PEA9lAWB0q%2FepN4VmmGwOHQCTMZ0bu7PXsv3dKcRDLd6cRrw2Lv%2F6tMva06rBg0OYlTjecPfGuGyJXeLkTahTyD9T0X56NA4%2BFZDkW%2FU7FAWwsqMojkRos0%2F%2BcFtxpFc1JJvNnWKpkMNnZpMcGOqUBDqqHxRckJkN%2F%2FrafI7IJnzQxlSTmhgnD1Fi5neYoTwRXG0U58Up9VBupDt0vZdh549bLs%2F0biwd%2FF6VwkBe%2FqShJDkanrVuxYU1iM9e5JFoCArUy3hfFbvR1EFFkqpeCyqaSIKYKzAORS6kxjKXPS5C2jPkxsqVPRqgqiUbY7VFpHZLWDMZjKCwD6mrD6G0GXZA2Fqw9vqBI5QvbnB2MLNa6X%2F7f&X-Amz-Signature=b32c4150f2d16cd8b0de58abdaefe7b9e9ecd7f9d230c9ae128f3d29c4fa57ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

