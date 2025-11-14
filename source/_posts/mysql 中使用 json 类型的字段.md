---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PAFPKIZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFXh55A4nlPYVtmZW14iqNcIqE10Vcoqiox%2F0Vy6HanaAiEA7zxFGd6FOK2Q6bTsnq%2FpWE3lBs0vN04wO0C0C9pM7eYq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDMrE5NvOUKYgWuSDoSrcAx%2BfuNTk13buDrCbgiQC8jTiqSv1LK6DowCDivSqbPXqQyYoP8LV2QB5rclOPU2581qqkWi4b%2BNTcU8qpD%2BFHLdJR8UOCzW8bVzoQ0ag1RfwrEi7ol%2F1c0EhgsMWWLUndiUbOthbDwlVd5DUGL1GLs%2B9lP7Y0YhDpEakam1oq%2FdxsJRaMYLqQppXVXdqO6ibNQE23OvAQOlUCtlUeVFHfl6dhq4Vj1VDLX57%2BAZPJGzjUYkD3w6SJqFYWWti5W1ngzdf2Lc0qnOETFUtkbaOLzS0HIWBtRQwNF8r0DY4O1QccGTsksv8BF%2B5urpniDLxAxHnuelMyRNJ37qorsnhSjwwIANpZxfkO6U9MZOHF%2FEdGkaFr9YC0axDOfmHEgyuG3rDjAF2RHOtS5Vz%2FdxMSnpilE9HFfRcczn75ePIJETj3USI4MtKXVCEb%2FkHkP1HHcVDdSMB3l8ebiYvwXlSEhJXwUYY8lxBMO3jUNWCiXNf%2BLswTp2inAQapr09zyeTsYkKcNBqHy1KMz6l%2BMwnHSvqylJffQjV9LSkWZmUsGwY3eNkO6kWb4TQjt2pDlNz4cAIUS7lUKLOhNxyBU1ifwywU5cZJNPmr404bWZ%2FpahMp1AeetnUTm5MP5HLML7d3MgGOqUBsCvBmMogOCYinJWApmwsnZRFxBFEY%2Bes793CkmmAryXXE6eQ%2FOyeKpOoA9QcCycYd9AgXa%2FvOpQnN4LUlS1Adjmvc5KxNVN0ZRwXlHqtI4QQCXpUD1YhZWa1Xw4StRUVGlXq32BURKMwFaad3rvlEiK1ySHc1kyvddUyTZy8aRdPj65hOz610eeM7thrX8Du7%2BNUSGvcXNSADdiktYbUi5ZWyouU&X-Amz-Signature=c54ebc4bd38ae537349e76f10c066ed79b895f493d969518277baf928e59b8dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

