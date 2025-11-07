---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFFSJLBY%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGatPj0rTfbJdQZhWR7i05WApef6o4cpT9oHEJnggpoSAiAkcFwOMc1HHozeykrLHkXxfN9BFM9YKcTClsd7r7D%2FuyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMoru4WSR%2BtdQGHAjsKtwDCsakW6hgncYVmP96ffT%2Bub2Okmvdw7eBcp%2B%2FYc5UnkrbTpRPm0oZBjKBxWcV2sK6VDM6AaL5d15suMBZxrOLeJIdeluyKLWuJMHM%2BsTtsoJensV3sSDRgD5f4uciyfol%2BxANKHTAGh6m%2B1po1PnSxB%2Fgc10kV5pZfgAPFGjqSButJAOQut6jnWTBGoqeG%2FbNKj6iXIZ2M%2F%2Bkcx9YHmoEx9y1CQxSMSutRYeHxYUJmhSmOVbLZnPLZxNgU4DXU0Ws%2FK%2FocXS0ZF%2FsKD9XG1IBDpW8iJR%2BEE6exjJsRft51fwaY7AovrXafbEQ9sYnpe4DeLm4KvHywIGhMRZ0F7ipD%2Bnv%2Bq%2FWa4pLe6duPQi59GM9W4yMESHzVMwg8v725PTjkq79a%2BnLS0C1DnOpZW0l%2Bq3t1UTHDQ33crdnHX6TnoxRfeR%2F3%2F0YpGw9muIs6lQYssRgUtm99v2uZP7n6VBRNg3yUOEUarufLm77CCZAuZM9i85ErXKdoAxu%2FMRx1WqQcl2GYWXRqzl381G8yvnqyfZN9%2F43gAF3uECZI4JA5qKFT0V%2BFs2aue2AfsYiovfAKsHko6v6zWpQTWddcKruzaYA1VH1RrcmrjetLmMEhlShWz00IhxTqgVJIfUwsIq4yAY6pgHrhpvJQ%2FDncOTzsKcmV18Wq73gOzwKc0ju6YA%2BhccyYZ5EaDbshIUnIblqe%2Bx%2B%2Frz5cb63LcWBipNHi4EaN4HWlnRhOtNzQsxCQmR97qxx5edAsNu%2FyAJP9v8UxewU5jL%2BGa1MhRb%2F6RS8e5GVBN54RfXVt%2FwQsh3pHiZN1PgOn4ACNcKzZVC9fKMIPKigL%2BoUHhFnHLU%2BHDlszjn22UPKtZ8HjlQk&X-Amz-Signature=3b4ba3209b5a64acb2c21b66c41de8bab6df2543a859d74fd87f448873b4dad8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

