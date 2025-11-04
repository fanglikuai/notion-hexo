---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTIVALW6%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjZ4%2Fp5Agvt680hABeAnyv9nqn4C1ge5taYkOfMSlv9AiEA3VgtaZ3%2BqqgT%2BPkNV3QI3BXO1Dp8CH8TlrSN7PeRZyYq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGCLH%2B6mZe%2FSP4RKISrcAy7CTnTAj8ZLuld793aOnE6IWE4b5qghN6U6ZxJHNZkasBsZabXua%2BEkZJcL4pWUqpG8uDmLAObejrBunsE2Gblt1VQAKOKoFJUCaLvITzVxR3WLHXvg%2Fq0UI%2FEeRiFTb9cOZHHexaZjI3Nf8%2BboKEvwhzNCDuwgzklD2fNCDWWxXZJ1ewIZESPUtzPW2CaxWKjyNFgMqKmmBvxWIwJyXNIaNN0U307VNROOPik3V%2FSSrVDmtQi%2BwF6OETFGlFwyXynmUeXGLPXtSbSyzcQ93babdHoY%2BjrGLoBuwN5%2BrQ%2Bf3wiXzZ87wSQZdVJsyF1vxvfySvV41Wub0D9lKzDAuRfTFcdmtx%2BCTOv4DAbQ%2B%2Fn%2B69PP2tr964agM9Ny%2BS0NKLV2MP4BDhvGeTWkzTeLMsqllneNhIlZFUClvlwJadK6Kgh7IS592WXQ33sbSTi4P0bLimh8YyJyLJ0d1jCPdBTsgKJcnilHMsJS%2FufI7mlBdd7NzHmOl7CvnH2C0uvBJ2YvKkTUnDIjcxp5JluVCUlRVLWdUKd9FVmfjBLIRurqnCs2M3wvV86EPrXJqvinqHT2c33udYO%2BVkMwG8dAXFgqfj8RcSZh033fJgNMJ0qeuV%2FuNzHkZc%2FG32TqMPLmqcgGOqUB5TfPRjMvw2qm9ejiW0Afww8Nnim1HizuFTf2ja2wezRvUm08U1lAL1oqkEVqMWpPh5lmVObkNpYiOkzA6N0VzfrocXcCalfEFelbjQ6EAj%2BLoPROlCAtieBTY9eg98W8FEh65Ct5ln%2Fe1syDec9dJgtzuZMouJ1H%2FZyvXUs7XpdBJIHt%2FNNOFaaQkJRj5zGco0%2BrucM4kYjF7c7DAjqYGPJHFZS%2F&X-Amz-Signature=46e8eb9fc96ad623475f9884a75cc1afd91a2a63d81f5c36d8d613a5d0818b01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

