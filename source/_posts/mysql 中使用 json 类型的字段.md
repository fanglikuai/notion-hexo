---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEGIDGXG%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T140101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC5Yeoi4k5V1m844BSJenzT0llBVTMMZjmukZqniObWEQIgHAoCtxGMidnLETBXkBLkBi5E%2BQmwGUV7IpHA7H6Q1kYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAeEHxmwRllSVa%2FZbircA0IoEmM33UMP1pK7c%2BkFXuxR06J4yhXgxc4LeRq%2F46bOzJFEJUbWE0qVV10cgsJ0QI6GqvQbQqRNCVgFIRUS%2FpLfJzok8yuxVjjDNuVTSSeMhp0ctvHZQslv9r1CraF7EkzcR57IHG81LimGoVty8NMqoyiTlvxxmymByzUOqG6nHZDiWRY8eVPPWpQofwHZgVEDx5N1jlyraceyLHAqMwnJ48BoKiBlUqOA4pwlxhUwC8qdHSURZmURP%2BcQQ%2BZik%2BYRvhQxfzLsQVaMLEL29Ykp%2BgQMwX9extYTxaN6rmJcavTrIFTM0GNI0egJLRA1b7R1qnk7N4AjaLmkgY1GpJN2Ijn5Z%2BWixFTO4tOjfF17usaEv%2BhonU1%2BRe%2BzvVrSLTO3OIredU8g0dZ1397o8F%2FNWjJ5A6Mx04feXMsdcvEd3FDMkHDUKinYnB82dWogcwS2CZV4P3BSDgGhj3CobOnKYmly11yuI7oX%2BBxTLrm%2FuxIOQJBzJJkQwuyyDY543RguIOHi35LTh8TRNYedBMKNEc5RELxYLMfy4pCAzXBV6zhyNkSrqSQNXpm2wIUsDZaHz%2FZ3QJVt1tHmlCfvGGySu%2FDV9QFwnrH4Ud%2BjVlY8GLHObeFu7lR4utIbMJj2l8gGOqUBGeh54rigMc7wkMoDUgA5QFka1BgpLrKV64JVJ55hZ2VgiNqfG1T7FeDjGFcKxdo53Hr1ir0ut1aBWIvG1AdBMBWJPSkYB4bO%2Bdox9J7sDFsE5lFC62ziOaUSrEB5uIxPhLpJVwn1HRhOmoo8QcomYsnt95sSGxQJQ9ZDj5hjggQZ9UTm6PLnI%2FeXV4y1k4y4%2BqE4ek1ySMoMJ00%2BuLB8WE3pbdPL&X-Amz-Signature=263cb0fa7b45368dfa80e2d9a8b5af558ca77625793db35f879247da77cb8955&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

