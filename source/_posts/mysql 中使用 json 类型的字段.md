---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U36DDQW6%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIH9pxv%2FVqRkKWNL7%2B7CjUBNsLAdnILHePfo4gsKJuK9pAiEAoTpZpRG4SNw4h9cDQiw5aThfRS6%2FnJ7ICm0JeOzvK2UqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfQQhTp4YhzNfNwaircA8BPtPggnuM8o%2FNgQgMYKmJd8zE1%2BH49%2FOy%2Fv%2BF65YyWP29bEoK5wDxnSCwYP3scln29uWU4RByrimX5vDoUB2Hc3puUThXYHEPthcC8CspSmBx7uK0bZAobCCIV3j0ZgBm5C1FdXzjovqZ%2BIoow2FJpf%2FO40eZrEa84zzvLvElS89OwOsaANounzPbLRTNQwHiBSsri0Ze3E3bRRv9TQgArSX5wEK%2FXVR6XiXrNNdGDh3Upk4XWhIdYNCwDsmjVmPdaopUHKAfJaT0O64QEoZQ24I5kvlNpdyR2SpBDz0MtZlgD6iityyu0ArXublZpSM87gMOq9ETuK1NiyO1iKFO69jY5PSBT2OsZi5rfVlNwTDZrAsT1uTK%2BjHgwcyjd8qFWIuDLbQY1K%2Fd4kGS8VS9UP5FYZlSyuhJvjS5xrkIsLK1RtAMyQkxn1HBM0cUYb32hFB4ujpcGbA3L1uVD6AbK1OqggHb2aZUQ9U5Uv2kNORjE04DqNRMSQXzdXdEoM9hBQ%2FpJF7eaPWJ7rVqjSgCi4nvNW9Ob38cxmAoSu5ysP7G1a4sQqTs%2BSNB5oGwjCAcALMFTUcGdTMewyYfLRF5I56VKWX9YdX8o6VH%2FZMyFYtsur86FtwlEkMKoMMC22ccGOqUB%2FVWNmUuJ4NyuRqgLw%2FtiugYif6eVG%2BYv53MNxnRbOrofkeYDIVgef0lyjHdDL7GiHPnQ7bJTVBA2QbKn7iQEZ1QqluzyXSv51dYyhv6GCvr1ojHVIGPukOWMw%2BL%2B71a%2FdQMhzxD2oQBVUXY35Nk46lrqLJX5DdI5TZmGRq4jOqbAEFeQjKalz%2BENoGFT4scJYvarq1n4WoIfyuNvKlGzaiOzcIPJ&X-Amz-Signature=4838f1c957f01945011e6b4b6cb3e8beedd7eb82de786bc471c432dd9c89da86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

