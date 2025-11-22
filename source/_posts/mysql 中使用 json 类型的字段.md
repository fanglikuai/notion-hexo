---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XU7R5ZAU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHJqH1Up1qYDgFjdxJ%2BU%2BLQNvbMTz3yRyTuSaoQHPYy7AiEAvlslvcuydaRhipj%2BPuKHSmpse4v0w3KC1%2F7QX2QLsMIq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDPH%2BaC1xdGFJvAnk7SrcA3uYcftsIW%2B%2BintdC9KvSQ1E%2Bj3hNBlOP%2FUHeIgQvTYmeKTMkjFJ5RwfAXGi79Ve87w4KVCzZBxyEjqETLlTQAur%2FJ%2BAid3NR%2Bp9V%2BAnHQnnUnN36Kt2mbPJzxpN1rpGTjibq%2B7JsZiPJyt4ErCm%2BbeXVl7Gvwqa7f1O%2BpLs6AkfVrNKeA9uuqTmRALCuycpB08uevQGUDE4y3hhNwCfj6uWxHckZvVzvS%2BDPBtVbG5gGl89%2FISdpBGuldOHnDfSG9EXg7FAPepaPb830I9f70ZIjif7G9kzsRSOACSVOsgPolGFNprLI4rcFWbtvz94QTBxyJpNHYoHHwn78PMrJj4tcJlCbhhXuhtwHR5d6WlYVaTcwPIAxKqiV1jsQilJhkVXwjf1kW05DzDAyDQ85t9D2ZBzGTBtyEVulz3lp%2FUx5sYIbV2jyN5LiIZv1Ke06aY2Wo%2F1YrcZBecIX9r4TcBXxDKVEKM8lvCVN7MILw53ND8oa1kRpufz81qeap2lNVeStvYEETJOpHDpp4JLC7brJflpfV%2BSzl5mxiJvdUImroxWo%2BKZNBNyt4iV7V7uYHf1MLA8gH22xTmNDUlsjTvtVGFuZczU8GzqwIfszQGL1h4uRKdgo2%2B9GzumMNPKhMkGOqUBsmw%2FtsXFhE198NZVbwvN7rrEMsDfwDBVvyfARUMKeVkJcpw9TKGJzCFxLFunkaaf7M9TqxLpzE9QiA%2BEBADY4SSn5lXx%2FMajmgmlP61cKHwGKYJYdfsk3T3YmA0XBmsgrCzVNwuI0vJHN8PC2bOp1JV3Nj%2BNoxhZxXcjFiNeiwNPYSFjgJUgsF47l%2BU5jhVH5zv8cd%2FvZESEu8EuD9vZGn%2BuI8xU&X-Amz-Signature=4e3413a83bf73b2ba8ffd0f0843acdded233bbb9317350f08745d2705cad0aaa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

