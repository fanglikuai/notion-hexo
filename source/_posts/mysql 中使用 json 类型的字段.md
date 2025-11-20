---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPMRYXDL%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCsbDj1Si5uiM9XDe0klFrH3c5q6XLLgh0%2BIhNeYnNRPAIgLNLnzc0sL4tUASPYauI3OM6%2FpwC9PfQq1mRFUUa%2F3n4qiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3VGnHB4px8jRe7ryrcAxWCK58Q13rOl5f0j4V1pELGJuZgDfA0SaBxEhjWMsaZZ1fpW%2F8j7ClOpDoFqeG3FhCrxx0A%2FkzY%2FhOifyuqFRH2MxSVl32jVm00L1M%2FJoi52zXrGyB%2BIMJU3kcPepCmrlraAa13vIGHTyl%2FPS%2BJ%2FCSwXwwkADEX6ZcIIl%2BGnusrjrTr%2B%2FlowR0w3X5DWonY5HSEcfN6oc4q8AA7uIdcXjyRtG8%2FkHIffERIdD3ZnLnFvJkumiruW7%2F78RPV23A69b%2BUrbHQIUiCJuLK35aVXoSCa2OD9miO%2B31S5ww65NTC%2BQgd8u%2FseAKa2wiCRku%2BybQntR207vJu4MnJKxlUuFP6uOYW%2FyuHMVMmKEJpoogvGleXxWD6zlmRsLv8uWJv7e3xuualaPArStm1j1rQp6TLqbzltX7Ac0kgtMY%2FqbbiZUrJgTKrXy%2FHyUY7fWQctQ329rrWGyBcWGWaMg5JMM3tcrUigI7eobAi7UARL7NBT8bSIFMQxA8QMxP4fiA2QUr2vQkCo7JL0z1uHQr1r%2BmzikWgjLmxyr3QqstUJ%2FoaDXzhuDtpqXimQlTdCYaAeVvP0i8K%2FCqsRTUoJ5hq0CdaujWztZh5MJ0Hn9%2Bq7ZxBXfkWfoPN99AVS2rnMPLR%2B8gGOqUBBLnHVNC7xVOJn66URvZujQJORI5R8Havmf%2BCxHDG4C%2FfUtRQ3NxkwEDpm0Fj%2BVj57SNLYvBozN3ll9b4LdFW3hnGVDocXb3EUogQuoS7RsA9L8HBWug%2BOUxDhhY8zJFjVsOhbD0wy59B1hdHgRD3dStVG2hdoS9BrTMxlOPqaCM70KUhn%2F0yk5oxR94G1wF8XkP6d8%2BUwTrLBJ1bRc1TFIt2qyrq&X-Amz-Signature=35b96fd970ace4b71d8ad11e7563bbe61616c8969d638d18acd98882363eabc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

