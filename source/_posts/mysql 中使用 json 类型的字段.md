---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DJ2JLY7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCWOvtfJ%2BxuREM2h0i4nQWGsBBkYlYaD0no2JvDJMM4wQIhAOrzriEiFH7pix%2B7f%2BC6o7kGox24pjhssvJT2uF0SXD%2BKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwHIvkf%2BuX7xjo0vEq3ANMUfJFkRK3fWdWCp1QgQv6tfCUWXa19nxmlwYWQuX6zKeMn%2FqcblnZ%2BX87ZP8cazjo1lazLpTwzJVbPigpcYHvF49yQ%2BKSqht%2FXz6BwDeicgpJZn7u2zyRsyO%2FQEyjc4t8ud1BFQUN0Hww4KacHFFi6CauBQfTvD%2FxW2suN4XlNKe41eDz83QujotfGL6Hy7dXvUVaCnISmrjULBy1rI%2FihXlxX4g7x2PZ9USHubHG7Jzji0vJOVXtmMm5S%2FjswtvA6eJ%2FxZ%2B1cDWZ8c3H9b6vBHpFqa9gdiYLGM3GKI%2BVgCbTpX00y5NJYN65oN3Blt%2BFHUhS8sRK%2BbegXFZlHr7pjlm7syBEO05AOomk%2B59QFjR%2BJ43FsVMOIG2zo%2FTh%2BJupqwb9LP3%2B%2BxiYZpX7C4fLjGGtLJqYN0GYlpvYWRDKsZeu4vdKfbhLv4%2FzgOFLPdcbDnl0sCb6ok1J58E%2B%2BcLxswIt9%2B5tJqaDFnN%2BTII7OWjq%2BN%2BYyfAna9UTEF4kmybWuGtIIQoys%2F02dZMknkgB3LI25RXxpVQesSSXQkl7ZSqxHEHSvjtOxivqFfS7kKTvjddVBmz5L62fVT5WK%2BMcdam%2F%2FWjtT1v6PkfIRppzeCwNiGiHQjKyaqCcojDS46bGBjqkAToRxeDloxwO2KVOxBXoJYnHrLi5cm4FSNHA8FWUNCmGAiRrLgngjIon5RR91y1wtQkIJMi%2FZHccErp7TsHbMdhJc0r%2Bm5AcHVe8N43vHHpt0Y37g80cKwY2ZdmIOtUXmtZF%2FmBWw5tYBPwRk6sBuzQLEqKqdpoew946O%2FOGbRVGjgMm7RssoEGRPOpTM91wbC40ChL3VlcOG06vcExAqSw3k6dN&X-Amz-Signature=707c5bfc0f7a523ca0bbef217e3a7e0827f609f7c0acda3b4bba1880ae64cdab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

