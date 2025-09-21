---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIKVY5HG%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T080110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCusF4A8DahVLko8qltw66InqYDeIaoAGIWxgf6iT0t%2BAIgVUxgMHlNHbRUqos%2FLDzn7Qo0GC1jg7%2BStO5tBAMeStYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB7eHeT4wf71YDDCkCrcA6deQAlvS8s%2BmjyGYM3etwBITw5ORyhMsoJTnXuQ%2Bw7sbNRlp%2BG1EMPYgP%2BmNCGxyzfdoY22%2B0bLez0yckFDNO2mQw%2B%2FoaAMEWopeaJtrfr2hEsdeCX8TfVYe%2BCZ8fCsQifScxeb6ox7%2Bu6nq%2BtJXFoCQZ97wxK4jkaJJ6dd%2BWkXI111yitr1kNz7hMfNtEkXf%2BIjEP7xALIdG5Oute2p6E%2BBh4%2BkN8wnqiyYgXFN7utWquy%2BksmYlOExWcqtDVDjJWGh%2BSUsSZMKkU5yH0HC3eKlRUgDvCWTlj4rXH2hcXZn4r%2BJmkBfcGizG9o9xMPReRfy1FZrcTo795iloDvrnB7UkQ4nZlVXUY1fi6CxG1pQDi8HnKYoCoHg5T4jboeRxooZnWjYpaDeqJ%2FPhR5Fed%2B8IsF1Q8%2BzNjFXWbaNC8VAdlTLEBeB4ZRr9qgyNRlGf0qoctTcypR5h0sk%2Fmz%2BZTzRsnjLfyeFxwC1Khcyp223%2BrkNMZh0lt3vxtdf92gX5JfA1kKJ4a4G53Zqt9LxSZSashXw5k6Zr7rn5bpVUMUprRl4XlxBSmI3qEuVbYkB5XvghZG8Kg%2BcXQ85uJ3Bt9G0nluSy%2FTEJkTGf9Qo8Aw0YA0iIR9AW6QjreQMN%2F%2BvcYGOqUBF9Q9L5WpAIkGhXkTbhubjVbK6%2BUgEBm4Rw5B9YzWSfSYO2ne485bWFVb2UWqhhLuo8qWY5UobFHav3XeAUC5PXjzS1iK43cCjdYh%2BojUUxXqT42NdNI6FzHA%2Fs2gaasnKqISU6dzGqTv9VpdQpA8NliQUkX%2FhVGRzXkRGe13RQntUHklM1o1x3AhnMVDxKHcYY2gr9AtOhf0fJyF5jOOsCS%2FXebT&X-Amz-Signature=87eeea2fb34cf312e2e952d382aeaed113f584a3acd5eeaeb3c202dc7ea004a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

