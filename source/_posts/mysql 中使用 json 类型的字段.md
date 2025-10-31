---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7ANN7B7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDMT0mkuCdKtxFOx1Spcnn3uRfmW6Kkp8cHGh%2Fwwkbb%2FgIgbtQeBw1RgyTlCtPMj4ZgHfCnvrOhmA1QDTTl%2B5iFYEgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGiFym9BKN84MX5zMSrcAwa%2B%2Bfwgh13I9bKNTGd5t5YYMW%2BbWN4Tp8ES4mEnx35DrWASPCK0bqREoAuPabHvEOfJwAKjhWnDBG%2BtreRrzjjK%2FGZrHkmo%2Bjs0wuwXDeQAqxgaiOSBnpLlVfHkxr9YSw2xniB2ywLQHRYmW8FF3X0Ujlv8dCe0zg7%2By2Lrw6Jwrief69SptaNvON0jeXPxBU9PaEKWqTEXNjSqP7WUvmZl2g0vKHCY%2Fw4XUI4KlyepdG2Azx%2FFVBQokt73KKVU%2FmNDIM4iOb9gZ8uTinPyqa2BhHMZPNdBEIKDy7kJlvMDQ%2BBqRoKVaDcA3riTRooqxNTeTBvoFhzJQZhSzU7rnRJRFH7mUNmpTFNaJp%2Fi%2BFQFm%2BTwPaNn7acD8QXEHnMAnuJZ6jHRvxL4un8IvyAu1eyIg%2BMkKstb7EhOEKr7zNMVVLxQWLg1XqqFtG4irbNlOTge5kLCpLxLxMQf3LtzVsnxrUjUBQyZQAp8KBiuNbMq1HvutDinKn%2BMpSfQfLPoIV9PXW5pCQgTfO3rbLLjEaTvFsdhzd8%2BAmSF41ZwsDnyQ5uGrRjt1oFtdVTLjoJIUqmruIgtN%2BZww88XXNmEPuc9osqgWDrLJ%2BAw6bSYUxqf3xApPDvIYLZ0RmxQMJbxj8gGOqUBquuyyqM3plWFpG7UYHC7nRW1xKfVc38u2dFL8%2F2rNc7WrHizvF0C8FjhPwymSJo7JaS8ElX10TokJKtghOX%2BKkR5%2FaRa7noWCfJZk9kz64Iamf4uYGtUkclz7da%2FdmC%2BRjEqLNgtOzod5Mw9WhsieAcPMhh62hrzd5zElcbP7rn9o5IUY%2BT8%2BxoiTRYQxJ1%2BgjVr6YkIF0S%2BxteLhYM%2FpEYwFSHG&X-Amz-Signature=549f3f47584d23668d7fd9ac027d5b2f74d28b04c4a98c809d0d61532ec2217a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

