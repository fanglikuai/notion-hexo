---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKK4T7KD%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE7PBIQyzfQ4ok25InepgAgVEqFvWqmgHhMI2CmNUZieAiB1FLSzZpsmg9LvpubdeWLTVQ9EQatfbitlOJ%2FbiRh7OSqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6FQJnC22ooVOKUweKtwDz%2BHjMRuzOVJW%2BWNzbUr%2BZDoylTVIC0z91TJOUvxzOeo2VspEs4ZRljRauv1TBgTVnzbPEGhF6I2m58qm4GsKAwpXVnyyMuUkAG6Flh11AQoM196x3ln98fySFl0oCx%2BzQYkobEm5whmqMmdrkK62XrE0I9JrJ8Y5glf24UtpW9%2FeShvKNGpAsYjS5ZzqG61SZpP9M6sLM7UZ5oNGiPEaPcFnwexoVlqxgaZNXJfEG%2F3%2FNUjXnn1q6Z0WBts4JjdP9nvdsr8QxqlnhTqZ%2BpG6k5%2BC%2FkxDoE28De6MPY1X59DAeR3XrJaGGa8NAG%2BnnsbxlD96DQv1eebVsTT7ybdDZif6QLOxMTGGVKqIRLKY7qEeX4QZIiNvkD59TJVzeR5ImMFDcuyAiw2tUnFgdDrGuosdjOqebep%2BXMNFTSm43FGHkwZuiTHH9oHL9iNuDuuc72pd6KLHAsruqkb4GxcH0m%2Bby9f1B6l9Ct5yUdvvq0R8YrlMBdUrOTSqsxl4y5T9y6EqSeItIAzqrWfiSyHrwAddKqNsYHzGOS%2Bn9OGMYxJrlA0R7gU88rZe9uCxQfunGBTnE4wJq%2BZ%2FTe59KG8ZbLVNE%2BXtd5lBbQMuaRkZDrzKXAE51kSTp0V%2Fwbgw%2FIq9xgY6pgEmX4XhSLU36CXXy9LfMUPkrCfHUsnfO08zvhWCuIvzpDJnHZcmcCIPIbCN9TcululF9Kt%2B%2FnP4URgHgVh1WWMRqCP%2BWFQxhw6XRqF%2Ba80Ssc9PBvBceu%2F%2FJFGo99xogNvzpox5fwEttuFTh2rJcwCs5GO5B519I71nWZ%2B%2BHWGumU3K75B2A7uPQWTPbu2Qf8lXpGfOpxCoXsGANzRuYwgyuFN%2Ftn3Y&X-Amz-Signature=c582ee6b90310a3a5bee6f2f0291bc29076c3fa46f4de76a0b0fc995a8c62bd6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

