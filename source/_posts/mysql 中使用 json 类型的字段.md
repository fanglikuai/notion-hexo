---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QL34LJNO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDBr0mDCdxOt8yg8SWF0alHKcbngywBapK2lBtcNAowUgIgIMoNIkgmJ043%2F6LxxdAB90bMBEMAw4CACGqMfq7fiNgq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDECLajrhsLhsv5WAHircA6KAuGvVRYNPfcUgnQvL%2BHsSZLHNnk%2Brkm5tERWpa4slXGNnlJdZ2%2FKjMMD9bGVoxhPD50Utt60GLDwnt0WaxRVPZrDBlnRZbKGSF6FWWRaftZpYcpvk%2Fp320%2BN3eqmzvlPYQ7MU7yvPLP9Heo%2FmMD%2FJkLxxjP314QtAAL6fQD5jlnwfl9DWO1PUfDMe6GKDQh0KhqbgqyIZa8UXKFCO1EAyJG1mhKhhjAvbddLTiXKOYmaEqsAuU88dWKwq4C2AzBKIZpYbhRk%2BzU4734KDcjHpHM3peAZyWZuGw0dpQCofW25HF7q2oXQJZdS4Xnyt5LKE3ecjG8DARwE9MXFx4rZS3rtjX8A1kR5L2Izam7K2AT7hdWEEo%2FrXafl397JG3eG3YJCubLEYhB4c0NkFyj%2BVbztOch7GsPGpCOIapbKp5CYLpdNRkpORP5s4vTABD6uCg794MQUMOwDwcJw9rBD8KRpz6MEHMHCK8Q65DdJneoXlHWiTf5ufkhvIXgX6u6wiBoPouYQhNSv77%2FIG7H9X053T%2FntSnNhJ6QUx%2FNKWWOlDCtzdT7jIj9NaQKHwSrz0sLVCKrlaPLz3BngYG2ZM%2FAzTs2VG3qJzYWqUR93hCux1pwOW1BtXhBbhMN%2Bi%2BMYGOqUBSG0cZ4%2BE3%2BuPHVHpRg0iq1M%2B99IkHQ9hbt2bg%2BBWAkdT0Qa37VEkQ6RGejd3D3pn22vIjvm%2F64yZkNrfNAv9Z%2BVRfDEBFjhQxomhDmK76SuiCjG5W%2BdNSak5OQ5J6e9IeElZVLXeNYFT8zAOdvJzOD5lE%2BJDXy3kPuPKbvf%2BJ8q1p7TKKGgZ9j9GOyJNcmL7GpiILUF%2Bj12d%2Fld6h%2FBzmiSV0GSv&X-Amz-Signature=3f53fba4949d79a7176a6e7b33aaf7b14062279a091862cd4574af6f50b64645&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

